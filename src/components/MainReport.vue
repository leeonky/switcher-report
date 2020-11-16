<template>
    <div>
        <table>
            <thead>
            <tr>
                <td>名称</td>
                <td>功率</td>
                <td>常载功率</td>
                <td>
                    <el-button @click="sortCircuit" type="text">相</el-button>
                </td>
                <td>用途</td>
                <td>操作</td>
            </tr>
            <circuit-row :circuit="newCircuit"></circuit-row>
            </thead>
            <circuit-row-list :circuits="circuits"></circuit-row-list>
        </table>

    </div>
</template>

<script>
    import CircuitRow from "./CircuitRow";
    import CircuitRowList from "./CircuitRowList";

    export default {
        name: 'Report',
        components: {CircuitRow, CircuitRowList},
        data() {
            return {
                dialogVisible: false,
                newCircuit: {},
                circuits: [{}, {}]
            };
        },
        methods: {
            sortCircuit() {
                this.circuits = []
            }
        }
    }
</script>

<style scoped>
</style>

