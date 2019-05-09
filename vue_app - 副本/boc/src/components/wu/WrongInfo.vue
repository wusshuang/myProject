<template>
    <table>
        <tr>
            <td>编号：</td>
            <td colspan="2">{{list.did}}</td>
        </tr>
        <tr>
            <td>订单uid：</td>
            <td colspan="2">{{list.uid}}</td>
        </tr>
        <tr>
            <td>订单oid：</td>
            <td colspan="2">{{list.otheruid}}</td>
        </tr>
        <tr>
            <td>状态：</td>
            <td colspan="2">{{list.state}}</td>
        </tr>
        <tr>
            <td>支付凭证：</td>
            <td colspan="2">
                <img :src='"upload/"+list.dealurl' style='width:100%'>
            </td>
        </tr>
        <tr>
            <td>匹配时间：</td>
            <td colspan="2">{{list.matching | timeFilter}}</td>
        </tr>
        <tr>
            <td>类型：</td>
            <td colspan="2">{{list.todo}}</td>
        </tr>
        <hr>
        <tr>
            <td>
                <button @click='force'>强制执行</button>
            </td>
            <td>
                <button @click='recover("待"+list.todo)'>订单恢复</button>
            </td>
        </tr>
        <tr>
            <td colspan="2">
                <button @click='del'>删除该订单</button>
            </td>
        </tr>
    </table>
</template>

<script>
import {Toast} from 'mint-ui'
export default {
    data(){
        return {
            list:{},
            did:this.$store.state.did
        }
    },
    created(){
        this.getWrongInfo()
    },
    methods:{

        del(){
            let url='Deletedeallist';
            let postData=this.qs.stringify({
                did:this.did,
                n:3
            })
            this.axios.post(url,postData).then(result=>{
                if(result.data.code==1){
                    Toast('删除成功！')
                }
            })
        },
        getWrongInfo(){
            let url='Succeed';
            let postData=this.qs.stringify({
                did:this.did
            });
            this.axios.post(url,postData).then(result=>{
                this.list=result.data[0];
                if(this.list.dealurl==''){
                    this.list.dealurl=='menu1.png'
                }
            })
        },
        force(){
            let url='dealOver';
            let postData4=this.qs.stringify({
                count:this.list.count*1.15,
                uid:this.list.uid,
                oid:this.list.otheruid,
                todo:this.list.todo
            })
            this.axios.post(url,postData4).then(result=>{
                this.recover('交易完成')
            })
        },
        recover(val){
            let url1='dealError'
            let postData=this.qs.stringify({
                did:this.did,
                state:val
            })
            this.axios.post(url1,postData).then(result=>{
                if(result.data.code==1){
                    Toast('操作成功！')
                }
            })
        }
    }
}
</script>

