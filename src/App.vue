<template>
    <div>
        <input id="charName" @input="onInput" placeholder="Enter a name to search for in here..."/>
        <table>
            <tr>
                <th>Name</th>
                <th>Height (cm)</th>
                <th>Mass (kg)</th>
                <th>Created</th>
                <th>Edited</th>
                <th>Planet Name</th>
            </tr>
            <CharacterList :textInput="textInput" :characters="characters"></CharacterList>
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
            return { characters: [], textInput: "" }
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
                }
            },
            onInput() {
                this.textInput = document.getElementById('charName').value;
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