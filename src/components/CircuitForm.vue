<template>
    <div>
        <el-form :model="circuit" label-width="80px" ref="form">
            <el-form-item label="名称">
                <el-input v-model="circuit.name"></el-input>
            </el-form-item>
            <el-form-item label="功率">
                <el-input type="number" v-model.number="circuit.power"/>
            </el-form-item>
            <el-form-item label="常载功率">
                <el-input type="number" v-model.number="circuit.normalPower"/>
            </el-form-item>
            <el-form-item label="相">
                <el-select v-model="circuit.phase">
                    <el-option label="R" value="R"></el-option>
                    <el-option label="G" value="G"></el-option>
                    <el-option label="B" value="B"></el-option>
                </el-select>
            </el-form-item>

            <el-form-item label="用途">
                <el-input v-model="circuit.purpose"/>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
    export default {
        name: "CircuitForm",
        props: {
            circuit: {
                type: Object,
                default: () => {
                }
            }
        }
    }
</script>

<style scoped>
</style>