<template>
    <tbody>
    <circuit-row :circuit="circuit" :key="circuit.id" v-for="circuit in circuits"></circuit-row>
    </tbody>
</template>

<script>
    import CircuitRow from "./CircuitRow";

    export default {
        name: "CircuitRowList",
        components: {CircuitRow},
        props: {
            circuits: {
                type: Array,
                default: () => []
            }
        }
    }
</script>

<style scoped>

</style>