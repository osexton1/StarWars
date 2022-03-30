<template>
    <CharacterListItem
    v-for="character in characters"
    :key="character.name"
    :character="character"
    :textInput="textInput"
    @openModal="openModal($event)"
    >
    </CharacterListItem>
</template>

<script>
    import CharacterListItem from './CharacterListItem';

    export default {
        data() {
            return { isModalVisible: false }
        },
        name: 'CharacterList',
        components: {
            CharacterListItem,
        },
        props: {
            characters: Array,
            textInput: String
        },
        methods: {
            openModal($event) {
                let planet = $event;
                this.$emit('showModal', planet);
            }
        }
    }
</script>
