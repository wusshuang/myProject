<template>
    <div>
         <mt-header title="交易大厅" fixed  class="mt-header wid"></mt-header>
         <Footer></Footer>
         <div class="deal-tit">当前交易价格 ¥{{pic}}</div>
         <div class="deal-btn">
             <button class="btn-buy" :style="{background:(btn.one?'#aae2de':'')}" @click="deal('one')" data-id="1">买入</button>
             <button class="btn-sell" :style="{background:(btn.two?'#aae2de':'')}" @click="deal('two')" data-id="2">卖出</button>
         </div>
         <hr>
         <div class="deal-form">
            <div class="login-item">
                &nbsp;&nbsp;<p> 数量：</p>
                <input type="text" name="count" v-model="count" placeholder="0~200之间整数">
            </div>
            <div class="login-item">
                &nbsp;&nbsp;<p> 单价：</p>
                <input type="text" name="price" v-model="price" placeholder="请输入买入单价">
            </div>
            <button class="deal-btn1" :data-id='0' @click="Adeal($event,'1')">申请{{submit}}</button>
         </div>
         <hr>
         <div v-show='btn.four'>
             <h4>我的待交易列表:</h4>
             <table>
                <tr class="tr1 bg">
                    <td>编号</td>
                    <td>单价</td>
                    <td>数量</td>
                    <td>总价</td>
                    <td>匹配时间</td>
                    <td>状态</td>
                    <td>操作</td>
                </tr>
                <tr v-for='(item,i) of list' :key='i'>
                    <td>{{item.number}}</td>
                    <td>￥{{item.price}}</td>
                    <td>{{item.count}}</td>
                    <td>￥{{item.totle}}</td>
                    <td>{{item.matching | timeFilter}}</td>
                    <td>{{item.state}}</td>
                    <td>
                        <button class="btn" :data-id='item.did' @click='lookdeal'>点击查看</button>
                    </td>
                </tr>
            </table>
         </div>
         <div v-show="btn.two">
            <h4>等待——买入列表:</h4>
            <table>
                <tr class="tr1 bg">
                    <td>编号</td>
                    <td>单价</td>
                    <td>数量</td>
                    <td>总价</td>
                    <td>状态</td>
                    <td>操作</td>
                </tr>
                <tr>
                    <td colspan="6">
                        <input type="text" placeholder="请输入订单编号搜索" v-model="search" class="search-input">
                        <button class="search-btn" @click='searchList'>搜索</button>
                    </td>
                </tr>
                <tr v-for="(item,i) of buyList" :key='i'>
                    <td>{{item.number}}</td>
                    <td>￥{{item.price}}</td>
                    <td>{{item.count}}</td>
                    <td>￥{{item.totle}}</td>
                    <td>{{item.state}}</td>
                    <td>
                        <button class="btn" :data-id='item.did'  :data-uid='item.uid' :data-count='item.count' @click='Adeal($event,"2")'>点击卖出</button>
                    </td>
                </tr>
            </table>
         </div>
         <div v-show="btn.one">
            <h4>等待——卖出列表:</h4>
            <table>
                <tr class="tr1 bg">
                    <td>编号</td>
                    <td>单价</td>
                    <td>数量</td>
                    <td>总价</td>
                    <td>状态</td>
                    <td>操作</td>
                </tr>
                <tr>
                    <td colspan="6">
                        <input type="text" placeholder="请输入订单编号搜索" v-model="search" class="search-input">
                        <button class="search-btn" @click='searchList'>搜索</button>
                    </td>
                </tr>
                <tr v-for="(item,i) of sellList" :key="i">
                    <td>{{item.number}}</td>
                    <td>￥{{item.price}}</td>
                    <td>{{item.count}}</td>
                    <td>￥{{item.totle}}</td>
                    <td>{{item.state}}</td>
                    <td>
                        <button class="btn" :data-id='item.did' :data-uid='item.uid' @click='Adeal($event,"2")'>点击购买</button>
                    </td>
                </tr>
            </table>
         </div>         
         <div v-show='btn.three'>
             <table>
                <tr class="tr1 bg">
                    <td>编号</td>
                    <td>单价</td>
                    <td>数量</td>
                    <td>总价</td>
                    <td>状态</td>
                    <td>操作</td>
                </tr>
                <tr>
                    <td>{{orderList.number}}</td>
                    <td>￥{{orderList.price}}</td>
                    <td>{{orderList.count}}</td>
                    <td>￥{{orderList.totle}}</td>
                    <td>{{orderList.state}}</td>
                    <td>
                        <button class="btn" :data-id='orderList.did' @click='Adeal($event,"2")'>点击交易</button>
                    </td>
                </tr>
            </table>
         </div>
         <div class='affirm' v-if="aff==1">
             <input type="password" placeholder="请输入六位数字的交易密码" v-model="dealupwd">
             <hr>
             <button @click='cancel'>取消</button>
             <button class='mybtn' @click="affirmupwd">确认</button>
         </div>
         <div class='affirm' v-else-if="aff==2">
             <p>注意：</p>
             <p>确认匹配后必需在一小时内完成收付款，投机者封号处理！</p>
             <hr>
             <button @click='cancel'>取消</button>
             <button class='mybtn' @click='affirmdeal'>确认</button>
         </div>
    </div>
</template>

<script>

import {Toast} from "mint-ui"
import Footer from "@/components/Footer"
export default {
    components:{
        Footer
    },
    data(){
        return {
            pic:'',
            count:'', //数量提交
            price:'', //单价
            search:'', //编号
            deposit:'', // 余额
            dealupwd:'',
            did:'',
            aff:0, //确认框
            btn:{
                one:true,
                two:false,
                three:false,
                four:false
            },
            submit:"买入",
            buyList:[],
            sellList:[],
            orderList:{}, //订单搜索
            list:[],
            buycount:'' ,//待买订单数量
            rank:''
        }
    },
    created(){
        this.getuser()
        this.getDealb()
        this.getDeals()
        this.nowpic()
        this.getme()
    },
    methods:{
        Adeal(e,n){
            if(this.rank=='未认证'){
                Toast('您还没有实名认证通过！')
                return
            }

            let id=e.target.dataset.id;
            let uid=e.target.dataset.uid;
            // console.log(typeof(e.target.dataset.count))
            if(e.target.dataset.count){
                // console.log(123)
                this.buycount=Number(e.target.dataset.count);
                if(Number(e.target.dataset.count)>this.deposit){
                    Toast('余额不足！')
                    return
                }
            }
            if(uid==sessionStorage.getItem('id')){
                Toast('不能匹配自己订单！')
                return
            }
            this.aff=n;
            this.$store.commit('Did',id)
            this.did=this.$store.state.did;
        },

        lookdeal(e){
            let id=e.target.dataset.id;
            this.$store.commit('Did',id)
            this.$router.push('Succeedb')
        },

        getme(){
            let url='Getdealing';
            this.axios.get(url).then(result=>{
                if(result.data.length>0){
                    this.list=result.data
                    this.btn.four=true
                }else{
                    this.btn.four=false
                }
            })
        },

        affirmdeal(){
            let time=new Date().getHours();
            if(time<9 || time>20){
                Toast('交易开放时间9:00~21:00')
                return
            }

            this.aff=0
            let url='GetOrderInfo';
            let postData=this.qs.stringify({
                did:this.did,
                otheruid:sessionStorage.getItem('id'),
                count:this.buycount
            });
            this.axios.post(url,postData).then(result=>{
                if(result.data.code==1){
                    Toast('抢单成功，请在一小时之内完成收付款确认！')
                    this.$router.push('Succeedb')
                }else{
                    Toast('该订单已被匹配！')
                }
            })
        },

        cancel(){//取消确认
            this.aff=0
        },

        affirmupwd(){ //交易密码查询
            if(!this.dealupwd){
                Toast('交易密码不能为空！')
                return
            }

            let url='Dupwd';
            let postData=this.qs.stringify({
                dealupwd:this.dealupwd
            })
            this.axios.post(url,postData).then(result=>{
                if(result.data.code==1){
                    this.dealupwd=''
                    this.aff=0;
                    this.applyDeal()
                }else{
                    Toast(result.data.msg)
                }
            })
        },
        //this.$store.commit('newNum',6)
        getuser(){
            let url='Self';
            this.axios.get(url).then(result=>{
                this.rank=result.data[0].rank;
                this.$store.commit('deposit',result.data[0].deposit);
                this.deposit=this.$store.state.depo;
            })
        },

        nowpic(){
            //获取当前交易价格
            let url='Pic';
            this.axios.get(url).then(result=>{
                this.pic=result.data
            })
        },
        
        deal(eee){
            this.btn.one=false;
            this.btn.two=false;
            this.btn.three=false;
            this.btn[eee]=true;
            if(this.btn.one==false){
                this.submit="卖出"
            }else{
                this.submit="买入"
            }
        },

        applyDeal(){
            
            let count=parseInt(this.count);
            if(this.submit=='卖出'){
                if(count>this.deposit*0.85){
                    Toast('您的余额不足!')
                    return
                }
            }
            
            if(count<1 || count>200){
                Toast('请输入1~200间整数')
                return
            }

            let time=new Date().getHours();
            if(time<9 || time>20){
                Toast('交易开放时间9:00~21:00')
                return
            }

            if(!count){
                Toast('提交数量不能为空！');
                return
            }

            if(!this.price){
                Toast('提交单价不能为空！');
                return
            }

            if(this.price>this.pic || this.price<(this.pic-0.3)){
                Toast('不能超过当前交易价格！')
                return
            }
            
            let url='Applydeal?';
            let postData=this.qs.stringify({
                count:count,
                price:Math.floor(this.price*100)/100,
                state:this.submit,
                totle:Math.floor(count*this.price*100)/100
            })
            this.axios.post(url,postData).then(result=>{
                if(result.data.code==1){
                    Toast('订单提交成功，待匹配！')
                    // this.$store.commit('deposit',this.deposit-this.count);
                    this.getDealb()
                    this.getDeals()
                    this.count=''
                    this.price=''
                }else{
                    Toast('网络忙，订单提交失败，请稍后再试！')
                }
            })
        },

        getDealb(){//待买列表
            let url='GetBuyDealList';
            this.axios.get(url).then(result=>{
                this.buyList=result.data
            })
        },
        
        getDeals(){//待卖列表
            let url='GetSellDealList';
            this.axios.get(url).then(result=>{
                this.sellList=result.data
            })
        },

        searchList(){//提交订单
            if(1){
                Toast('功能暂未开放，敬请期待！')
                return
            }
            
            if(!this.search){
                Toast('订单编号不能为空！')
                return
            }

            let url='GetOrder?number='+this.search;
            this.axios.get(url).then(result=>{
                if(result.data.length==0){
                    Toast('该订单不存在！')
                }else{
                    this.deal('three')
                    this.orderList=result.data[0]
                    this.search=''
                }
            })
        }
    }
}
</script>

<style>
    .affirm{
        width: 300px;
        height: 160px;
        background: rgba(0,0,0,0.6);
        padding: 20px;
        position: absolute;
        border-radius: 20px;
        left: 50%;
        margin-left: -150px;
        top: 50%;
        margin-top: -80px;
        z-index: 100
    }
    .affirm>p{
        color: rgb(235,235,235);
        font-size: 18px !important;
        font-weight: 500
    }
    .affirm>input{
        font-size: 15px
    }
    .affirm>button{
        margin-left: 20%;
    }
    .deal-tit{
        text-align: center;
        background: rgb(170, 226, 222);
        font-weight:600;
        padding: 10px;
        margin:10px 0;
    }
    .deal-btn{
        display: flex;
        justify-content: space-around;
        margin: 20px;
    }
    .btn-buy,.btn-sell{
        padding:5px 50px;
    }
    .deal-btn1{
        margin-left: 80%;
        background: rgb(170, 226, 222);
    }
    .deal-form{
        padding: 5px 20px;
        margin: 10px 0;
    }
    .bg{
        background: rgb(170, 226, 222) !important;
    }
    .search-input{
        width: 74% !important;
        border-radius: 20px !important;
        margin-bottom: 0 !important;
        margin: 10px 0;
        border: 2px solid rgb(170, 226, 222) !important;
    }
    .search-btn{
        width: 24%;
        border-radius: 20px !important;
        background: rgb(170, 226, 222);
        margin: 15px 0;
    }
</style>
