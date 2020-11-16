<template>
    <tr>
        <td>
            <el-input v-model="circuit.name"></el-input>
        </td>
        <td>
            <el-input type="number" v-model.number="circuit.power"/>
        </td>
        <td>
            <el-input type="number" v-model.number="circuit.normalPower"/>
        </td>
        <td>
            <el-select v-model="circuit.phase">
                <el-option label="R" value="R"></el-option>
                <el-option label="G" value="G"></el-option>
                <el-option label="B" value="B"></el-option>
                <el-option label="-"></el-option>
            </el-select>
        </td>
        <td>
            <el-input v-model="circuit.purpose"/>
        </td>
        <td>
            <el-input v-model="circuit.type"/>
        </td>
        <td>
            <el-input v-model="circuit.bit"/>
        </td>
        <td>
            <el-button :key="action.id" :type="action.type" @click="doAction({action: action.action, circuit: circuit})"
                       v-for="action in actions">
                {{action.label}}
            </el-button>
        </td>
    </tr>
</template>

<script>
    export default {
        name: "CircuitRow",
        props: {
            circuit: {
                type: Object,
                default: () => {
                }
            },
            actions: {
                type: Array,
                default: () => []
            }
        },
        methods: {
            doAction(arg) {
                this.$emit("circuit-change", arg)
            }
        }
    }
</script>

<style scoped>
</style>