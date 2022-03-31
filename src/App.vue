<template>
    <div>
        <h1 v-if="currentPage < 1">
            All Star Wars Characters
        </h1>
        <h1 v-else>
            Star Wars Characters
        </h1>
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
            <CharacterList @showModal="showModal($event)" :textInput="textInput" :characters="sortedTable"></CharacterList>
        </table>
        <p v-if="currentPage < 1">
            <button @click="nextPage">Next</button>
        </p>
        <p v-else-if="currentPage > maxPages - 1">
            <button @click="prevPage">Previous</button>
        </p>
        <p v-else>
            <button @click="prevPage">Previous</button>
            | {{ currentPage }} |
            <button @click="nextPage">Next</button>
        </p>
        <Modal v-show='isModalVisible' @close='closeModal' :planet="planet"/>
    </div>
</template>

<script>
    import CharacterList from './components/CharacterList';
    import Modal from './components/Modal';
    import axios from 'axios';

    const ROOT_URL = 'https://swapi.dev/api';

    export default {
        name: 'App',
        components: {
            CharacterList,
            Modal
        },
        data() {
            return { characters: [], textInput: "", currentSort: "created", currentSortDir: "asc",
                     pageSize: 10, currentPage: 0, isModalVisible: false, planet : "", maxPages: 0 }
        },
        methods: {
            async fetchCharacters() {
                axios.get(`${ROOT_URL}/people`).then(response => {
                    this.characters = response.data.results;
                    this.maxPages = ~~(this.characters.length/10) + 1;
                    const nextPage = response.data.next;
                    this.fetchCharacters_(nextPage);
                });
            },
            async fetchCharacters_(url) {
                if (url) {
                    axios.get(url).then(response => {
                        this.characters = this.characters.concat(response.data.results);
                        this.maxPages = ~~(this.characters.length/10) + 1;
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
                        this.characters[char].homeworld = [this.characters[char].homeworld, response.data.name];
                    })
                }
            },
            nextPage() {
                this.currentPage += 1;
            },
            prevPage() {
                this.currentPage -= 1;
            },
            showModal($event) {
                this.planet = $event;
                this.isModalVisible = true;
            },
            closeModal() {
                this.isModalVisible = false;
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
                        if (this.currentSort === 'homeworld') {
                            if (a[this.currentSort][1] < b[this.currentSort][1]) return -1 * modifier;
                        if (a[this.currentSort][1] > b[this.currentSort][1]) return 1 * modifier;
                        return 0;
                        } else {
                            if (a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
                            if (a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
                            return 0;
                        }
                    }
                }).filter((row, index) => {
                    if (this.currentPage < 1) {
                        return true;
                    } else {
                        let start = (this.currentPage-1) * this.pageSize;
                        let end = this.currentPage * this.pageSize;
                        if (index >= start && index < end) return true;
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
    * {
        color: #feda4a;
        font-family: 'Pathway Gothic One', sans-serif;
    }

    table {
        margin: auto;
        border-collapse: collapse;
        width: 80%;
        text-align: center;
        position: relative;
    }

    th {
        background-image: url('../public/SWBackground.jpeg');
        position: sticky;
        top: 0;
        margin: 1em;
        padding: 0.5em;
    }

    input {
        background-color: transparent;
        border: 0;
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

    button {
        background-color: transparent;
        border: 0;
        font-weight: bold;
        font-size: 1.2em;
    }

    p {
        font-weight: bold;
    }

</style>