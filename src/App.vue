<template>
    <div>
        <input id="charName" @input="onInput" placeholder="Enter a name to search for in here..."/>
        <table>
            <tr>
                <th>Name</th>
                <th>Height</th>
                <th>Mass</th>
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
        margin: auto;
        border: 2px solid black;
        border-collapse: collapse;
        width: 80%;
        text-align: center;
    }

    th {
        margin: 1em;
        border: 2px solid black;
    }

    input {
        width: 75%;
        margin: auto;
        text-align: center;
    }

    div {
        text-align: center;
        margin: auto;
    }

</style>