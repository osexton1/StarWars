<template>
    <tr v-if="character.name.toLowerCase().includes(textInput.toLowerCase())">
        <td>{{ character.name }}</td>
        <td>{{ character.height }}</td>
        <td>{{ character.mass }}</td>
        <td>{{ character.created }}</td>
        <td>{{ character.edited }}</td>
        <td>{{ this.homeworld }}</td>
    </tr>
</template>

<script>
    import axios from 'axios';

    export default {
        name: 'CharacterListItem',
        data() {
            return { homeworld: "" }
        },
        props: {
            character: Object,
            textInput: String
        },
        methods: {
            async getPlanetName() {
                axios.get(this.character.homeworld).then(response => {
                    this.homeworld = response.data.name;
                });
            }
        },
        created() {
            this.getPlanetName()
        }
    }
</script>

<style scoped>
    td {
        margin: 1em;
        border: 2px solid black;
    }
</style>