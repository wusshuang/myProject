<template>
    <div>
        <mt-header title="财务明细" fixed class="mt-header wid">
            <router-link to="Myhome" slot="left">
                <mt-button icon="back"></mt-button>
            </router-link>
        </mt-header>
        <table>
            <tr class="tr1">
                <td>编号</td>
                <td>类型</td>
                <td>数量</td>
                <td>时间</td>
                <td>余额</td>
            </tr>
            <tr class="tr2" v-for="(item,i) of list" :key="i">
                <td>{{item.pid}}</td>
                <td>{{item.type}}</td>
                <td>{{item.count}}</td>
                <td>{{item.ptime | timeFilter}}</td>
                <td>{{item.depo}}</td>
            </tr>
        </table> 
    </div>
</template>

<script>
export default {
    data(){
        return {
            list:[],
            pno:0
        }
    },
    created(){
        this.getdeal()
    },
    methods:{
        getdeal(){
            let url='Getproperty?pno='+this.pno;
            this.pno++;
            this.axios.get(url).then(result=>{
                this.list=result.data
            })
        },
    }
}
</script>

