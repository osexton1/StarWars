<template>
    <div class="modal-backdrop">
        <div class="modal">
            <header class="modal-header">
                <slot name="header">
                    {{ this.$props.planet.name }}
                </slot>
            </header>
            <section class="modal-body">
                <slot name="body">
                    <p>Diameter: {{ printNumber(this.$props.planet.diameter) }}</p>
                    <p class="capitalise">Climate: {{ this.$props.planet.climate }}</p>
                    <p class="capitalise">Terrain: {{ this.$props.planet.terrain }}</p>
                    <p>Population: {{ printNumber(this.$props.planet.population) }}</p>
                </slot>
            </section>
            <footer class="modal-footer">
                <button @click='close'>Close</button>
            </footer>
        </div>
    </div>
</template>

<script>

    export default {
        name: 'MyModal',
        props: {
            planet: Object
        },
        methods: {
            close() {
                this.$emit('close');
            },
            printNumber(number) {
                if (number != "unknown") {
                    return Number(number).toLocaleString();
                } else {
                    return "Unknown";
                }
            }
        }
    }
</script>

<style scoped>
    .modal-backdrop {
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        display: flex;
        justify-content: center;
        align-items: center;   
    }

    .modal {
        box-shadow: 2px 2px 20px 1px;
        background-image: url('../../public/SWBackground.jpeg');
        overflow-x: auto;
        display: flex;
        flex-direction: column;
    }

    .modal-header,
    .modal-footer {
        padding: 1.5em;
        display: flex;
     }

    .modal-header {
        position: relative;
        border-bottom: 1px solid #feda4a;
        font-weight: bold;
        justify-content: space-between;
    }

    .modal-footer {
        border-top: 1px solid #feda4a;
        flex-direction: column;
        justify-content: flex-end;
    }

    .modal-body {
        position: relative;
        padding: 2em 1em;
    }

    button {
        background-color: transparent;
        color: #feda4a;
        border: none;
        font-weight: bold;
    }

    .capitalise {
        text-transform: capitalize;
    }
</style>