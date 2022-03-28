<template>
    <CharacterListItem
    v-for="character in characters"
    :key="character.name"
    :character="character"
    :textInput="textInput"
    >
    </CharacterListItem>
</template>

<script>
    import CharacterListItem from './CharacterListItem';

    export default {
        name: 'CharacterList',
        components: {
            CharacterListItem
        },
        props: {
            characters: Array,
            textInput: String
        }
    }
</script>

<style scoped>

    tr:nth-child(even) {
        background-color: #a79f9f;
    }
</style>