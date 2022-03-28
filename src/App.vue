<template>
    <div>
        Hello There
        <table>
            <tr>
                <th>Name</th>
                <th>Height</th>
                <th>Mass</th>
                <th>Created</th>
                <th>Edited</th>
                <th>Planet Name</th>
            </tr>
            <CharacterList></CharacterList>
        </table>
    </div>
</template>

<script>
    import CharacterList from './components/CharacterList';

    export default {
        name: 'App',
        components: {
            CharacterList
        }
    }

</script>

<style scoped>
    table {
        margin: 1em;
        border: 2px solid black;
        border-collapse: collapse;
        width: 80%;
        text-align: center;
    }

    th {
        margin: 1em;
        border: 2px solid black;
    }
</style>