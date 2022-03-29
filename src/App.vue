<template>
    <div>
        <input id="charName" @input="onInput" placeholder="Enter a name to search for in here..."/>
        <table>
            <tr>
                <th @click="sort('name')">Name</th>
                <th @click="sort('height')">Height (cm)</th>
                <th @click="sort('mass')">Mass (kg)</th>
                <th @click="sort('created')">Created</th>
                <th @click="sort('edited')">Edited</th>
                <th @click="sort('homeworld')">Planet Name</th>
            </tr>
            <CharacterList :textInput="textInput" :characters="sortedTable"></CharacterList>
        </table>
    </div>
</template>

<script>
    import CharacterList from './components/CharacterList';
    import axios from 'axios';

    const ROOT_URL = 'https://swapi.dev/api';

    export default {
        name: 'App',
        components: {
            CharacterList,
        },
        data() {
            return { characters: [], textInput: "", currentSort: "created", currentSortDir: "asc" }
        },
        methods: {
            async fetchCharacters() {
                axios.get(`${ROOT_URL}/people`).then(response => {
                    this.characters = response.data.results;
                    const nextPage = response.data.next;
                    this.fetchCharacters_(nextPage);
                });
            },
            async fetchCharacters_(url) {
                if (url) {
                    axios.get(url).then(response => {
                        this.characters = this.characters.concat(response.data.results);
                        const nextPage = response.data.next;
                        this.fetchCharacters_(nextPage);
                    });
                } else {
                    this.fetchPlanet();
                }
            },
            onInput() {
                this.textInput = document.getElementById('charName').value;
            },
            sort(column) {
                if (column === this.currentSort) {
                    this.currentSortDir = this.currentSortDir==="asc"?"desc":"asc";
                }
                this.currentSort = column;
            },
            async fetchPlanet() {
                for (let char in this.characters) {
                    axios.get(this.characters[char].homeworld).then(response => {
                        this.characters[char].homeworld = response.data.name;
                    })
                }
            }
        },
        computed: {
            sortedTable() {
                return this.characters.slice().sort((a,b) => {
                    const numbers = ['height', 'mass'];
                    let modifier = 1;
                    if (this.currentSortDir === "desc") modifier = -1;
                    if (numbers.includes(this.currentSort)) {
                        if (a[this.currentSort] === 'unknown') {
                            return -1 * modifier;
                        }
                        if (b[this.currentSort] === 'unknown') {
                            return 1 * modifier;
                        }
                        return (parseInt(a[this.currentSort].replace(',','')) - parseInt(b[this.currentSort].replace(',',''))) * modifier;
                    } else {
                        if (a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
                        if (a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
                        return 0;
                    }
                });
            }
        },
        created() {
            this.fetchCharacters();
        }
    }

</script>

<style scoped>
    table {
        font-family: 'Pathway Gothic One', sans-serif;
        margin: auto;
        border-collapse: collapse;
        width: 80%;
        text-align: center;
    }

    th {
        color: #feda4a;
        margin: 1em;
        padding: 0.5em;
    }

    input {
        font-family: 'Pathway Gothic One', sans-serif;
        background-color: transparent;
        border: 0;
        color: #feda4a;
        width: 75%;
        margin: 1em;
        text-align: center;
    }

    div {
        text-align: center;
        margin: auto;
    }

    ::placeholder {
        color: #feda4a;
        opacity: 0.75;
    }

</style>