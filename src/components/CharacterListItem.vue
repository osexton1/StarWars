<template>
    <tr v-if="character.name.toLowerCase().includes(textInput.toLowerCase())">
        <td>{{ character.name }}</td>
        <td>{{ character.height }}</td>
        <td>{{ character.mass }}</td>
        <td>{{ character.created }}</td>
        <td>{{ character.edited }}</td>
        <td>{{ character.homeworld }}</td>
    </tr>
</template>

<script>

    export default {
        name: 'CharacterListItem',
        data() {
            return { homeworld: "" }
        },
        props: {
            character: Object,
            textInput: String
        },
        // created() {
        //     this.getPlanetName()
        // }
    }
</script>

<style scoped>
    td {
        font-family: 'Pathway Gothic One', sans-serif;
        color: #feda4a;
        margin: 2em;
        padding: 0.5em;
    }
</style>