<template>
    <div>
        <table>
            <tr>
                <th>Name</th>
                <th>Height</th>
                <th>Mass</th>
                <th>Created</th>
                <th>Edited</th>
                <th>Planet Name</th>
            </tr>
            <CharacterList :characters="characters"></CharacterList>
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
            CharacterList
        },
        data() {
            return { characters: [] }
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
                })
                console.log(url);
                }
            }
        },
        created() {
            this.fetchCharacters();
        }
    }

</script>

<style scoped>
    table {
        margin: 1;
        border: 2px solid black;
        border-collapse: collapse;
        width: 100%;
        text-align: center;
    }

    th {
        margin: 1em;
        border: 2px solid black;
    }
</style>