<template>
    <tbody>
    <circuit-row :actions="actions" :circuit="circuit" :key="circuit.id" v-for="circuit in circuits"></circuit-row>
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
        },
        data() {
            return {
                actions: [
                    {label: "删除", type: "warning", action: "delete"},
                    {label: "复制", type: "primary", action: "copy"}
                ]
            }
        }
    }
</script>

<style scoped>

</style>