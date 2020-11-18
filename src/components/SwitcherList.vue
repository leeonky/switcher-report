<template>
    <div>
        <table>
            <thead>
            <th>空开</th>
            <th>个数</th>
            <th>回路</th>
            </thead>
            <tbody>
            <tr :key="report.id" v-for="report in this.reports().concat(this.otherSwitchers)">
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
        data() {
          return {
              otherSwitchers: [{
                  switcher: "2PC63(漏保)",
                  types: ["G路分相", "R路分相", "B路分相"]
              },{
                  switcher: "4PC80",
                  types: ["总入户"]
              },{
                  switcher: "4PC63(漏保)",
                  types: ["工具间"]
              },{
                  switcher: "4PC40",
                  types: ["车库"]
              }]
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
                }).sort((a,b)=>{
                    let s1 = a.switcher;
                    let s2 = b.switcher;
                    if(s1>s2)
                        return 1;
                    if(s1<s2)
                        return -1;
                    return 0;
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
table {
    text-align: left;
    font-size: 12pt;
}
td {
    padding-right: 20px;
    padding-top: 5px;
}
th {
    padding-right: 20px;
}
</style>