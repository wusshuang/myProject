<template>
    <div>
        <mt-header title="交易中心" fixed class="mt-header wid"></mt-header>
        <br>
        <div class="banner">
            <img :src="'img/logo'+i+'.png'">
        </div>
        <!-- <mt-swipe>
            <mt-swipe-item>
                <img src="banner1.png">
            </mt-swipe-item>
            <mt-swipe-item>
                <img src="logo1.png">
            </mt-swipe-item>
        </mt-swipe> -->
        <br>
        <h4>&gt; 矿机商城</h4>
        <br>
        <div class="millList">
            <div class="listItem" v-for="(item,i) of millList" :key="i">
                <img :src='"img/"+item.img'>
                <p>型号 ：{{item.title}}</p>
                <p>租用价格 ：{{item.pic}}BOC</p>
                <p>产量/小时 {{item.compute}}</p>
                <mt-button size="large" :data-pic='item.pic' type="primary" class="mybtn" @click='rent'>立即租用</mt-button>
            </div>
        </div>
        <Footer></Footer>
    </div>
</template>

<script>

import Footer from "@/components/Footer"
import {Toast} from "mint-ui"

export default {
    components: {
        Footer
    },
    data(){
        return {
            millList:[
                {mid:1,title:"微型云矿机",img:"bmill.png",pic:10,compute:"0.0167"},
                {mid:2,title:"小型云矿机",img:"bmill.png",pic:100,compute:"0.1805"},
                {mid:3,title:"中型云矿机",img:"bmill.png",pic:1000,compute:"2.0139"},
                {mid:4,title:"大型云矿机",img:"bmill.png",pic:10000,compute:"22.2222"}
            ],
            deposit:'',
            i:1
        }
    },
    created(){
        this.getDe()
    },
    methods:{
        rent(e){
            let pic=e.target.dataset.pic;
            if(this.deposit<Number(pic)){
                Toast('账户余额不足！')
                return
            }

            let url='Rent';
            let postData=this.qs.stringify({
                pic
            })
            this.axios.post(url,postData).then(result=>{
                if(result.data.code==1){
                    Toast('提交成功！')
                    this.getDe()
                }else{
                    Toast('服务器出错！')
                }
            })
        },

        getDe(){
            setInterval(()=>{
                this.i++;
                if(this.i==3){
                    this.i=1
                }
            },1000)

            let url='Self';
            this.axios.get(url).then(result=>{
                this.deposit=result.data[0].deposit
            })
        }
    }
}
</script>

<style>
    .banner{
        margin:  0 0;
        height: 200px;
        overflow: hidden;
    }
    .banner img{
        width: 100%;
    }
    .millList{
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        padding: 4px;
    }
    .listItem{
        width:49%;
        text-align: center;
    }
    .listItem>img{
        width: 100%;
        display: block;
    }
    .listItem>p{
        color: rgb(0,0,0)
    }
</style>


