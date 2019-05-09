<template>
    <table>
        <tr>
            <td>
                <h4>证件：</h4>
            </td>
            <td class="utd" colspan="2">
                <img :src="'upload/'+uitem.cardurl" alt="">
                <!-- <img src="upload/card1.png" alt=""> -->
            </td>
        </tr>
        <tr v-show="toChild2">
            <td>
                <button @click="checkUser('2')">不通过</button>
            </td>
            <td>
                <button @click="checkUser('1')">通过</button>
            </td>
        </tr>
        <tr>
            <td>名字：</td>
            <td>{{uitem.uname}}</td>
        </tr>
        <tr>
            <td>编号：</td>
            <td>{{uitem.uid}}</td>
        </tr>
        <tr>
            <td>电话：</td>
            <td>{{uitem.phone}}</td>
        </tr>
        <tr>
            <td>状态：</td>
            <td>{{uitem.state}}</td>
        </tr>
        <tr>
            <td>支付宝：</td>
            <td>{{uitem.pay}}</td>
        </tr>
        <tr>
            <td>登录密码：</td>
            <td>{{uitem.logupwd}}</td>
        </tr>
        <tr>
            <td>等级：</td>
            <td>{{uitem.rank}}</td>
        </tr>
        <tr>
            <td>注册时间：</td>
            <td>{{uitem.rtime}}</td>
        </tr>
    </table>
</template>

<script>

import {Toast} from "mint-ui"
export default {
    name:'Ulist',
    props:['toChild','toChild2'],
    data(){
        return {
            Nodo:true,
            uitem:{} //用户信息
        }
    },
    created(){
        this.open()
    },
    methods:{
        open(){
            this.uitem=this.toChild
        },
        
        no(){
            this.Nodo=false
        },

        checkUser(a){
            let uid=this.uitem.uid;
            let url=`Checkcard`;
            let card;
            if(a==1){
                card=1
            }else if(a==2){;
                card=2
            }else{
                // 隐藏此版块
                this.Nodo=false;
                return
            }
            let postData=this.qs.stringify({
                uid:uid,
                card:card
            })
            this.axios.post(url,postData).then(result=>{
                if(result.data.code==1){
                    window.location.reload();
                    Toast(result.data.msg)
                }else{
                    Toast('木弄成，再试一次！')
                }
            })
        }
    }
}
</script>

<style>
    .utd{
        width: 80%;
    }
    .utd>img{
        width: 100%;
    }
</style>


