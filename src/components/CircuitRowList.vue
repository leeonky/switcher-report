<template>
    <tbody>
    <circuit-row :actions="actions" :circuit="circuit" :key="circuit.id" @circuit-change="copyOrDelete"
                 v-for="circuit in circuits"></circuit-row>
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
                    {label: "删", type: "warning", action: "delete"},
                    {label: "复", type: "primary", action: "copy"}
                ]
            }
        },
        methods: {
            copyOrDelete(data) {
                this.$emit("circuit-change", data)
            }
        }
    }
</script>

<style scoped>

</style>