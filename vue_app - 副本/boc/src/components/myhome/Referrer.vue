<template>
    <div>
        <mt-header title="推广中心" fixed class="mt-header wid">
            <router-link to="Myhome" slot="left">
                <mt-button icon="back"></mt-button>
            </router-link>
        </mt-header> 
        <div>
            <h4 class="ref-h4">&nbsp;&nbsp;▼ 我的推广链接</h4>
            &nbsp;&nbsp;&nbsp;&nbsp;
            <router-link to='Reg?boss=15036368031' id="link"> http://bitoc.applinzi.com/#/Log?boss={{phone}}</router-link>
            <br>
            <button id="copy" class="bg" @click="btnCopy">点击复制链接</button>
        </div>
        <div class="ab-tit">▼ 最新推荐奖励</div>
        <div class="ab-d1">
            <p class="ab-p">
                比特基金（BOC）平台优势  
                <span class="ab-span"> 
                    <br>
                    1、稀缺的发行数量， 比特基金初期发型量少。 
                    <br>
                    <br>
                    2、超高利润，简单操作 挖矿者直接服务于整合平台用户流量，工作量直接转化为收益，矿机算法能力强大。 
                    <br>  
                    <br>  
                    3、整合支付平台，资金变现速度快 在数字货币普及的大趋势下，我们的比特基金对接线上线下合作资源，。 
                    <br>
                    <br>
                    4、去现金化，比特基金除最初抢购矿机销售期间外，全程均只有币之间的兑换，每日的免费矿机限量发布，更是给予了零元创业人群也能打造无限财富的机会。</span>
            </p>
        </div>    
        <!-- <upload :uploadType="`head`" :imgWidth="`85px`" :imgHeight="`104px`" :imgUrl="imgUrl"
        @upload="getImgUrl"></upload> -->
    </div>
</template>

<script>
import {Toast} from "mint-ui"
export default {
    components:{
    },
    data(){
        return {
            phone:''
        }
    },
    created(){
        this.self()
    },
    methods:{
        btnCopy() {
            var url = document.getElementById("link").innerText;
            var input = document.createElement("input");
            input.value = url;
            document.body.appendChild(input);
            input.select(); // 选择对象
            document.execCommand("Copy"); // 执行浏览器复制命令
            input.style.display = "none";
            Toast("复制成功");
        },
        self(){
            let url="Self";
            this.axios.get(url).then(result=>{
                this.phone=result.data[0].phone;   
            })
        }
    }
}
</script>

<style>
    .ref-h4{
        margin: 15px 0;
    }
    #link{
        font-size: 10px;
        color: rgb(0, 0, 0);
    }
    #copy{
        margin-left: 60%;
    }
</style>


