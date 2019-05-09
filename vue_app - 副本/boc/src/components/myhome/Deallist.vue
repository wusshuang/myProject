<template>
    <div>
        <mt-header title="订单记录" fixed class="mt-header wid">
            <router-link to="Myhome" slot="left">
                <mt-button icon="back"></mt-button>
            </router-link>
        </mt-header>
        <table>
            <tr class="tr1">
                <td>编号</td>
                <td>单价</td>
                <td>数量</td>
                <td>总价</td>
                <td>状态</td>
                <td>时间</td>
                <td>删除</td>
            </tr>
            <tr class="tr2" v-for="(item,i) of dlist" :key="i">
                <td>{{item.number}}</td>
                <td>￥{{item.price}}</td>
                <td>{{item.count}}</td>
                <td>￥{{item.totle}}</td>
                <td>{{item.state}}</td>
                <td>{{item.rtime | timeFilter}}</td>
                <td>
                    <button class='mybtn' @click='drop' :data-id='item.did' :data-state='item.state'>撤销订单</button>
                </td>
            </tr>
        </table> 
        <br>
        <button @click="getdeal" class='page'>下一页</button>
    </div>
</template>

<script>

import {Toast} from 'mint-ui'
export default {
    data(){
        return {
            dlist:[],
            pno:0
        }
    },
    created(){
        this.getdeal()
    },
    methods:{

        getdeal(){
            let url='Getdeal?pno='+this.pno;
            this.pno++;
            this.axios.get(url).then(result=>{
                this.dlist=result.data
            })
        },

        drop(e){
            let state=e.target.dataset.state;
            let did=e.target.dataset.id;
            if(state=='待买入' || state=='待卖出'){
                let url='DeleteDealList';
                let postData=this.qs.stringify({
                    did:did
                })
                this.axios.post(url,postData).then(result=>{
                    Toast('撤销成功！');
                    this.pno=0;
                    this.getdeal()
                })
            }else{
                Toast('只能撤销待交易订单！')
            }
        }
    }
}
</script>

