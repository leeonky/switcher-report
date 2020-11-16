<template>
    <div>
        <table>
            <thead>
            <th>空开</th>
            <th>个数</th>
            <th>回路</th>
            </thead>
            <tbody>
            <tr :key="report.id" v-for="report in this.reports()">
                <td>{{report.switcher}}</td>
                <td>{{report.types.length}}</td>
                <td>
                    <span class="circuit-type" v-for="type in report.types" :key="type.id">{{type}}</span>
                </td>
            </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        name: "SwitcherList",
        props: {
            switchers: {
                type: Object,
                default: () => {}
            }
        },
        methods: {
            reports() {
                let flatSwitchers = {...this.switchers.R, ...this.switchers.G, ...this.switchers.B};
                let switcherReport = Object.keys(flatSwitchers).reduce((switcherMap, purpose)=> {
                    let switcher = flatSwitchers[purpose];
                    (switcherMap[switcher]=switcherMap[switcher]||[]).push(purpose)
                    return switcherMap
                }, {});
                return Object.keys(switcherReport).map(s => {
                    return {
                        switcher: s,
                        types: switcherReport[s]
                    }
                });
            }
        }
    }
</script>

<style scoped>
.circuit-type{
    background-color: #d3dce6;
    margin-right: 20px;
}
</style>