<template>
    <div>
        <hr>
        <el-row>
            <el-col :span="12">
                <table>
                    <thead>
                    <th>名称</th>
                    <th>功率</th>
                    <th>常载功率</th>
                    <th>用途</th>
                    <th>相</th>
                    <th>分类</th>
                    </thead>
                    <tbody>
                    <tr :key="circuit.id" class="circuit" v-for="circuit in circuits">
                        <td>
                            {{circuit.name}}
                        </td>
                        <td>
                            {{circuit.power}}
                        </td>
                        <td>
                            {{circuit.normalPower}}
                        </td>
                        <td>
                            {{circuit.purpose}}
                        </td>
                        <td>
                            <select v-model="circuit.phase">
                                <option label="R" value="R"></option>
                                <option label="G" value="G"></option>
                                <option label="B" value="B"></option>
                                <option :value="undefined" label="-"></option>
                            </select>
                        </td>
                        <td>
                            <input v-model="circuit.type"/>
                        </td>
                    </tr>
                    </tbody>
                </table>
                <hr>
            </el-col>
            <el-col :span="12">
                <table>
                    <thead>
                    <th></th>
                    <th>功率</th>
                    <th>常用功率</th>
                    </thead>
                    <tbody>
                    <tr :key="purpose" v-for="purpose in this.colValues('purpose')">
                        <td>{{purpose}}</td>
                        <td>{{sumBy("power", "purpose", purpose)}}</td>
                        <td>{{sumBy("normalPower", "purpose", purpose)}}</td>
                    </tr>
                    <tr>
                        <td>总和</td>
                        <td>
                            {{this.sum("power")}}
                        </td>
                        <td>
                            {{this.sum("normalPower")}}
                        </td>
                    </tr>
                    </tbody>
                </table>
            </el-col>
        </el-row>
        <el-row>
            <el-col :span="24">

                <table>
                    <thead>
                    <th>开关</th>
                    <th>回路数</th>
                    <th>功率</th>
                    <th>常用功率</th>
                    <th>回路</th>
                    </thead>
                    <tbody>
                        <tr :key="type" v-for="type in this.colValues('type')">
                        <td>{{type}}</td>
                        <td>{{countBy("type", type)}}</td>
                        <td>{{sumBy("power", "type", type)}}</td>
                        <td>{{sumBy("normalPower", "type", type)}}</td>
                        <td>{{circuitsBy("type", type)}}</td>
                    </tr>
                    </tbody>
                </table>
            </el-col>
        </el-row>
    </div>
</template>

<script>
    export default {
        name: "PhaseCircuits",
        props: {
            phase: {
                type: String,
                default: "未知"
            },
            circuits: {
                type: Array,
                default: () => []
            }
        },
        methods: {
            sum(valueField) {
                return this.circuits
                    .map(x => x[valueField])
                    .filter(x => x !== undefined)
                    .filter(x => x !== "")
                    .reduce((a, b) => a + b, 0);
            },
            sumBy(valueField, keyField, key) {
                return this.circuits
                    .filter(x => x[keyField] === key)
                    .map(x => x[valueField])
                    .filter(x => x !== undefined)
                    .filter(x => x !== "")
                    .reduce((a, b) => a + b, 0);
            },
            countBy(keyField, key) {
                return this.circuits
                    .filter(x => x[keyField] === key)
                    .length
            },
            colValues(key) {
                return [...new Set(this.circuits
                    .map(x => x[key])
                    .filter(x => x !== undefined)
                    .filter(x => x !== ""))]
            },
            circuitsBy(keyField, key) {
                return this.circuits
                    .filter(x => x[keyField] === key)
                    .map(x => x.name)
                    .join(", ")
            }
        }
    }
</script>

<style scoped>
    table {
        text-align: left;
        font-size: 12pt;
    }
    td {
        padding-right: 20px;
    }
    th {
        padding-right: 20px;
    }
</style>