<template>
    <div class="succ">
        <mt-header title="交易信息" fixed class="mt-header wid">
            <router-link to="Deal" slot="left">
                <mt-button icon="back"></mt-button>
            </router-link>
        </mt-header>
        <h4>订单编号：{{orderInfo.number}}：{{time}}</h4>
        <hr>
        <div class='info'>
            <p>单价：￥ {{orderInfo.price}}</p>
            <p>数量：{{orderInfo.count}}</p>
            <p>总价：￥ {{orderInfo.totle}}</p>
        </div>
        <div class='info' v-show="off.three">
            <p>卖方手机号：{{oList.phone}}</p>
            <p>卖方姓名：{{oList.uname}}</p>
            <p>卖方支付宝号：
                <u id="link1">{{oList.pay}}</u>
                <br>
                <br>
                <button id="copy" class="bg" @click="btnCopy('1')">点击复制支付宝号</button>
            </p>
        </div>
        <div class='info' v-show="off.four">
            <p>卖方手机号：{{orderInfo.phone}}</p>
            <p>卖方姓名：{{orderInfo.uname}}</p>
            <p>卖方支付宝号：
                <u id="link2">{{orderInfo.pay}}</u>
                <br>
                <br>
                <button id="copy" class="bg" @click="btnCopy('2')">点击复制支付宝号</button>
            </p>
        </div>
        <div class='info' v-show="off.five"> 
            <p>买方手机号：{{orderInfo.phone}}</p>
            <p>买方姓名：{{orderInfo.uname}}</p>
        </div>
        <div class='info' v-show="off.six"> 
            <p>买方手机号：{{oList.phone}}</p>
            <p>买方姓名：{{oList.uname}}</p>
        </div>

        <hr>
        <div class='info tell' v-show="off.one">
            <h4>如果您已完成付款：</h4>
            <br>
            <p>请及时上传支付凭证，然后短信或电话通知对方确认！</p>
            <p>如未按规定时间付款，封号处理！</p>
            <p>如通知对方后规定时间内仍未确认，对方封号！</p>
        </div>
        <div class='info tell' v-show="off.two">
            <h4>您的订单已完成匹配：</h4>
            <br>
            <p>请及时短信或电话联系对方付款！</p>
            <p>如对方已付款卖方未按规定时间确认收款，封号处理！</p>
            <p>如通知对方后规定时间内仍未付款，对方封号！</p>
        </div>

        <hr>
        <div v-show='show'>
            <p class="uploadp" v-show="off.one">上传成功,请通知对方确认！</p>
            <p class="uploadp"  v-show="off.two">对方已付款，过期未确认封号！</p>
            <img :src='dealurl' alt="" class='proveimg' >
        </div>
        <hr>

        <!-- // <form :action='"uploadFile?did="+did'  method="POST" enctype="multipart/form-data" @submit='uploadimg' v-show="off.one">
        //     <input type="file" name="mypic" class="upload" id='img'>
        //     <br>
        //     <br>
        //     <input type="submit" value="上传照片" class="submit">
        // </form> -->
        <div v-show="off.one">
            <input  accept="image/*" type="file" name="mypic" class="upload" id="imag">
            <br>
            <input type="button" value="上传照片" class="submit" @click="uploadimg">
        </div>
        <hr>

        <div v-show="off.one">
            <!-- <button class="mybtn sbtn" @click='error("1","订单已被冻结，待系统审核处理！")'>订单异常</button> -->
        </div>
        <div v-show="off.two">
           <!-- <button class="mybtn sbtn" @click='error("2","订单已被冻结，待系统审核处理！")'>交易异常</button> -->
            <button class="mybtn sbtn" @click='over'>确认收款</button>
        </div>
    </div>
</template>

<script>

import {Toast} from "mint-ui"
export default {
    data(){
        return {
            did:this.$store.state.did,
            show:false,
            dealurl:"",
            orderInfo:{},
            time:'',
            oList:{},
            off:{
                one:false,
                two:false,
                three:false,
                four:false,
                five:false,
                six:false
            },
            endtime:'',
        }
    },
    created(){
        this.getList()
    },
    methods:{
        over(){

            this.interval('2');  //清除定时器
            let url='dealOver';
            let postData4=this.qs.stringify({
                count:this.orderInfo.count*1.15,
                uid:this.orderInfo.uid,
                oid:this.orderInfo.otheruid,
                todo:this.orderInfo.todo
            })
            this.axios.post(url,postData4).then(result=>{
                this.error('4','交易完成！');
                this.off.two=false
            })
        },

        error(d,t){
            

            let state;
            if(d==1){
                state='买报异常'
            }else if(d==2){
                state='卖报异常'
            }else if(d==3){
                state='超时异常'
            }else{
                state='交易完成';
                this.endtime=0
            }

            if(this.endtime>0){
                Toast('交易时间未到！')
                return
            }
            let postData3=this.qs.stringify({
                did:this.did,
                state:state
            })

            let url='dealError';
            this.axios.post(url,postData3).then(result=>{
                Toast(t)
                this.time=t
            })
        },

        uploadimg(){
            let file1 = document.getElementById("imag").files[0];
            if(file1===undefined){
                Toast('请选择要上传的图片！');
                return
            }
            var formdata1=new FormData();// 创建form对象
            formdata1.append('mypic',file1,file1.name);// 通过append向form对象添加数据,可以通过append继续添加数据
            //或formdata1.append('img',file);
            let config = {
                headers:{'Content-Type':'multipart/form-data'}
            };  //添加请求头
            this.axios.post(`uploadFile?did=${this.did}`,formdata1,config).then(result=>{   //这里的/xapi/upimage为接口
                if(result.data.code==1){
                    Toast('上传成功！')
                    setTimeout(()=>{
                        this.$router.push('Deal')
                    },1500)
                }
            })

        },

        getList(){


            let url='Succeed';
            if(this.$route.query.did){
                this.did=this.$route.query.did
            }else{
                this.did=this.$store.state.did
            }
            let postData=this.qs.stringify({
                did:this.did
            });
            this.axios.post(url,postData).then(result=>{//此订单信息
                this.getListt(result.data)
            })
        },
        getListt(arr){
            this.orderInfo=arr[0];
            if(this.orderInfo.state=='交易完成'){
                this.time='交易已完成';
                this.show=true;
                this.off.two=false;
                this.off.one=false;
                this.off.three=false;
                this.off.four=false;
                this.off.five=false;
                this.off.six=false
            }
            let otherid=this.orderInfo.otheruid;//获取主动方个人信息
            let url1='Self?uid='+otherid;
            this.axios.get(url1).then(result=>{
                this.oList=result.data[0];
                if(this.orderInfo.state=='已匹配买'){
                    //判断订单类型 再判断当前用户身份
                    if(this.orderInfo.uid==sessionStorage.getItem('id')){
                        this.off.two=false;
                        this.off.one=true;
                        this.off.three=true;
                        this.off.four=false;
                        this.off.five=false;
                        this.off.six=false
                    }else if(otherid==sessionStorage.getItem('id')){
                        this.off.one=false;
                        this.off.two=true;
                        this.off.three=false;
                        this.off.four=false;
                        this.off.five=true;
                        this.off.six=false
                    }
                }else if(this.orderInfo.state=='已匹配卖'){
                    if(this.orderInfo.uid==sessionStorage.getItem('id')){
                        this.off.one=false;
                        this.off.two=true;
                        this.off.three=false;
                        this.off.four=false;
                        this.off.five=false;
                        this.off.six=true;
                    }else if(otherid==sessionStorage.getItem('id')){
                        this.off.one=true;
                        this.off.two=false;
                        this.off.three=false;
                        this.off.four=true;
                        this.off.five=false;
                        this.off.six=false
                    }
                }
            })

            this.dealurl=this.orderInfo.dealurl;
            if(this.dealurl!=''){
                this.show=true
                this.dealurl=`upload/${this.dealurl}`
            }else{
                this.show=false
                this.dealurl='img/menu1.png'
            }

            this.interval('1')
        },

        interval(y){
            let tt;
            let t=setInterval(()=>{
                tt=Number((3600-parseInt((new Date().getTime()-new Date(this.orderInfo.matching).getTime())/1000))/60).toFixed(2)
                this.time=`剩余确认时间 ${tt} 分`;
                this.endtime=tt;
                if(tt<0){
                    clearInterval(t);
                    this.error("3","订单已被冻结，待系统审核处理！")
                    this.time='该订单超时，已被冻结!'
                }
                if(y==2){
                    clearInterval(t);
                }
            },1000)
        },

        btnCopy(r) { //复制支付宝号
            let url;
            if(r==1){
                url = document.getElementById("link1").innerText;
            }else{
                url = document.getElementById("link2").innerText;
            }
            var input = document.createElement("input");
            input.value = url;
            document.body.appendChild(input);
            input.select(); // 选择对象
            document.execCommand("Copy"); // 执行浏览器复制命令
            input.style.display = "none";
            Toast("复制成功");
        }
    }    
}
</script>

<style>
    .succ>h4{
        padding: 10px;
        margin: 10px auto;
    }
    .info{
        padding:1% 3%; 
    }
    .info>p{
        /* color: rgb(165,42,42); */
        color: rgb(12, 97, 131);
        font-size: 17px;
        font-weight: 700
    }
    .info>p>u{
        color: rgb(0, 0, 0) !important;
        font-size: 18px !important
    }
    .tell>p{
        font-size: 14px;

    }
    .proveimg{
        display: block;
        margin: 0 auto;
        width: 80%;
    }
    .uploadp{
        margin-left: 20%;
        color: rgb(12, 97, 131);
        font-size: 19px
    }
    .sbtn{
        margin-left: 20%;
    }
</style>


