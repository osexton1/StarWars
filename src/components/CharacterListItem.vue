<template>
    <tr v-if="character.name.toLowerCase().includes(textInput.toLowerCase())">
        <td>{{ character.name }}</td>
        <td>{{ character.height }}</td>
        <td>{{ character.mass }}</td>
        <td>{{ formatDate(character.created) }}</td>
        <td>{{ formatDate(character.edited) }}</td>
        <td @click='openModal(character.name)' class="modal">{{ character.homeworld[1] }}</td>
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
        methods: {
            formatDate(entryDate) {
                const date = new Date(entryDate);
                const dmyFormat = date.toDateString();
                let hour = date.getUTCHours();
                let minutes = date.getMinutes();
                if (hour < 10) {
                    hour = "0" + hour.toString();
                }
                if (minutes < 10) {
                    minutes = "0" + minutes.toString();
                }
                return dmyFormat + " @ " + hour + ":" + minutes;
            },
            openModal() {
                this.$emit('openModal', this.character.homeworld[0]);
            }
        }
    }
</script>

<style scoped>
    td {
        font-family: 'Pathway Gothic One', sans-serif;
        color: #feda4a;
        margin: 1.5em;
        padding: 0.5em;
    }

    .modal {
        text-decoration-line: underline;
    }
</style>