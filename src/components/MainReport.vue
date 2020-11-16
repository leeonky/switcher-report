<template>
    <div>
        <el-input type="textarea" v-model="circuitsJson"></el-input>
        <el-button @click="loadCircuits" type="danger">加载</el-button>
        <el-button @click="report" type="success">保存</el-button>
        <hr>
        <table>
            <thead>
            <circuit-row :actions="actions" :circuit="newCircuit" @circuit-change="addCircuit"></circuit-row>
            <tr>
                <td>
                    <el-button @click="sortCircuit('name')" type="text">名称</el-button>
                </td>
                <td>功率</td>
                <td>常载功率</td>
                <td>
                    <el-button @click="sortCircuit('phase')" type="text">相</el-button>
                </td>
                <td>用途</td>
                <td>操作</td>
            </tr>
            </thead>
            <circuit-row-list :circuits="circuits" @circuit-change="copyOrDelete"></circuit-row-list>
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
                newCircuit: {},
                circuits: [],
                actions: [
                    {label: "添加", type: "success", action: "new"}
                ],
                circuitsJson: "[]",
                groupedCircuits: {}
            };
        },
        methods: {
            sortCircuit(key) {
                console.log(key)
                this.circuits = this.circuits.sort((c1, c2) => {
                    if (c1[key] > c2[key])
                        return 1;
                    if (c1[key] < c2[key])
                        return -1;
                    return 0;
                })
            },
            addCircuit(data) {
                this.circuits.splice(0, 0, {...data.circuit})
                this.report()
            },
            copyOrDelete(data) {
                this.$emit("circuit-change", data)
                if (data.action === "delete") {
                    this.circuits.splice(this.circuits.indexOf(data.circuit), 1)
                } else if (data.action === "copy") {
                    this.circuits.splice(this.circuits.indexOf(data.circuit), 0, {...data.circuit})
                }
                this.report()
            },
            report() {
                this.circuitsJson = JSON.stringify(this.circuits)
                this.groupedCircuits = this.circuits.reduce((map, current) => {
                    (map[current.phase] = map[current.phase] || []).push(current)
                    return map
                }, {})
                console.log(this.groupedCircuits)
            },
            loadCircuits() {
                this.circuits = JSON.parse(this.circuitsJson)
                this.report()
            }
        }
    }
</script>

<style scoped>
</style>

