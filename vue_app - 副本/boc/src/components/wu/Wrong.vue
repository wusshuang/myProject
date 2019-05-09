<template>
    <table>
        <thead> 
            <tr class="tr1">
                <td>订单编号</td>
                <td>订单uid</td>
                <td>订单oid</td>
                <td>状态</td>
                <td>时间</td>
                <td>类型</td>
                <td>查看</td>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(item,i) of WrongList" :key="i">
                <td>{{item.did}}</td>
                <td>{{item.uid}}</td>
                <td>{{item.otheruid}}</td>
                <td>{{item.state}}</td>
                <td>{{item.matching | timeFilter}}</td>
                <td>{{item.todo}}</td>
                <td>
                    <button @click="see" :data-id="item.did">查看</button>
                </td>
            </tr>
        </tbody>
    </table>
</template>


<script>
import {Toast} from 'mint-ui'
export default {
    data(){
        return {
            WrongList:[]
        }
    },
    created(){
        this.getWrong()
    },
    methods:{
        getWrong(){
            let url='Wrong'
            this.axios.get(url).then(result=>{
                if(result.data.code==-1){
                    Toast('没有异常订单！')
                }else{
                    this.WrongList=result.data
                }
            })
        },
        see(e){
            let did=e.target.dataset.id;
            this.$store.commit('Did',did);
            this.$router.push('WrongInfo')
        }
    }
}
</script>
