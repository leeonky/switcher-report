<template>
    <div>
        <el-row>
            <el-col :span="24">
                <div class="grid-content bg-purple-dark">
                    <el-input type="textarea" v-model="circuitsJson"></el-input>
                    <el-button @click="loadCircuits" type="danger">加载</el-button>
                </div>
            </el-col>
        </el-row>
        <el-row>
            <el-col :span="8">
                <div class="grid-content bg-purple">
                    <table>
                        <thead>
                        <circuit-row :actions="actions" :circuit="newCircuit"
                                     @circuit-change="addCircuit"></circuit-row>
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
                            <td>分类</td>
                            <td>P</td>
                            <td>操作</td>
                        </tr>
                        </thead>
                        <circuit-row-list :circuits="circuits" @circuit-change="copyOrDelete"></circuit-row-list>
                    </table>
                </div>
            </el-col>
            <el-col :span="16">
                <div class="report">
                    <phase-circuits :circuits="groupedCircuits[phase]" :key="phase.id" :phase="phase"
                                    v-for="phase in phases" :switchers="switchers[phase]"></phase-circuits>
                    <hr>
                    <switcher-list :switchers="switchers"></switcher-list>
                </div>
            </el-col>
        </el-row>
    </div>
</template>

<script>
    import CircuitRow from "./CircuitRow";
    import CircuitRowList from "./CircuitRowList";
    import PhaseCircuits from "./PhaseCircuits";
    import SwitcherList from "./SwitcherList";

    export default {
        name: 'Report',
        components: {CircuitRow, CircuitRowList, PhaseCircuits, SwitcherList},
        data() {
            return {
                newCircuit: {},
                circuits: [],
                actions: [
                    {label: "添加", type: "success", action: "new"}
                ],
                circuitsJson: "{}",
                groupedCircuits: {},
                phases: [],
                switchers: {R:{},G:{},B:{}}
            };
        },
        watch: {
            circuits: {
                handler: function () {
                    this.reports()
                },
                deep: true
            },
            switchers: {
                handler: function () {
                    this.reports()
                },
                deep: true
            }
        },
        methods: {
            sortCircuit(key) {
                console.log(key)
                this.circuits = this.circuits.sort((c1, c2) => {
                    let key1 = '' + c1[key];
                    let key2 = '' + c2[key];
                    if (key1 > key2)
                        return 1;
                    if (key1 < key2)
                        return -1;
                    return 0;
                })
            },
            addCircuit(data) {
                this.circuits.splice(0, 0, {...data.circuit})
            },
            copyOrDelete(data) {
                this.$emit("circuit-change", data)
                if (data.action === "delete") {
                    this.circuits.splice(this.circuits.indexOf(data.circuit), 1)
                } else if (data.action === "copy") {
                    this.circuits.splice(this.circuits.indexOf(data.circuit), 0, {...data.circuit})
                }
            },
            reports() {
                this.circuitsJson = JSON.stringify({circuits: this.circuits, switchers: this.switchers})
                this.groupedCircuits = this.circuits.reduce((map, current) => {
                    (map[current.phase] = map[current.phase] || []).push(current)
                    return map
                }, {})
                this.phases = Object.keys(this.groupedCircuits).sort()
            },
            loadCircuits() {
                let obj = JSON.parse(this.circuitsJson);
                this.circuits = obj.circuits
                this.switchers = obj.switchers
            }
        }
    }
</script>

<style scoped>
    .bg-purple-dark {
        background: #99a9bf;
    }

    .bg-purple {
        background: #d3dce6;
    }

    .grid-content {
        border-radius: 4px;
        min-height: 36px;
    }
    .report {
        padding:  30px;
    }
</style>

