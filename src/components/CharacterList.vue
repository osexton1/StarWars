<template>
    <CharacterListItem
    v-for="character in characters"
    :key="character.name"
    :character="character"
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
            characters: Array
        }
    }
</script>