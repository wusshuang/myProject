<template>
    <div>
        <mt-header title="矿机详情" fixed class="mt-header wid">
            <router-link to="Mill" slot="left">
                <mt-button icon="back"></mt-button>
            </router-link>
        </mt-header>
        <img src="img/zuan.png" alt="" class="loadimg">
        <div id="mat">
            <div>
                <span>0</span>
                <span>1</span>
                <span>0</span>
                <span>1</span>
                <span>0</span>
                <span>0</span>
                <span>1</span>
                <span>0</span>
                <span>1</span>
                <span>0</span>
            </div>
        </div>
        <div class="production">
            <p>累计产出：{{millo}}</p>
            <p>当前可领：{{(millo-mList.yetget).toFixed(8)}}</p>
            <button @click='get'>领取</button>
        </div>
    </div>
</template>

<script>

import {Toast} from "mint-ui"
export default {
    data(){
        return {
            millo:null, //累计产出
            millid:this.$store.state.mid ,//矿机id            
            mList:{} //矿机详情
        }
    },
    mounted(){
        this.can()  
    },
    created(){
        this.getmills()
    },
    methods:{
        get(){
            if(this.millo-this.mList.yetget<0.3){
                Toast('超过0.3个可领！')
                return
            }
            let getm=this.millo-this.mList.yetget;
            let url='Upyetget';
            let postData=this.qs.stringify({
                get:getm,
                mid:this.millid
            })
            this.axios.post(url,postData).then(result=>{
                if(result.data.code==1){
                    Toast('领取成功！')
                    this.mList.yetget=this.millo
                }
            })
        },

        getmills(){
            
            let url='GetMillInfo?mid='+this.millid;
            this.axios.get(url).then(result=>{
                this.mList=result.data[0];
                let time=new Date().getTime()-new Date(this.mList.mtime).getTime();
                let out;
                let timer;
                if(this.mList.type=='微型云矿机'){
                    out=Number(time*0.00000463/1000);                   
                    if(out>12){
                        this.millo=12.00
                        this.update(this.millid) 
                    }else{
                        timer=setInterval(()=>{
                            out+=0.00000463/10
                            this.millo=out.toFixed(8)
                        },100)
                    }
                }else if(this.mList.type=='小型云矿机'){
                    out=Number(time*0.00005015/1000);
                    if(out>130){
                        this.millo=130.00;
                        this.update(this.millid) 
                    }else{
                        timer=setInterval(()=>{
                            out+=0.00005015/10
                            this.millo=out.toFixed(8)
                        },100)
                    } 
                }else if(this.mList.type=='中型云矿机'){
                    out=Number(time*0.00055941/1000);                    
                    if(out>1450){
                        this.millo=1450.00
                        this.update(this.millid) 
                    }else{
                        timer=setInterval(()=>{
                            out+=0.00055941/10
                            this.millo=out.toFixed(8)
                        },100)
                    }
                }else{
                    out=Number(time*0.00617284/1000);
                    if(out>16000){
                        this.millo=16000.00
                        this.update(this.millid) 
                    }else{
                        timer=setInterval(()=>{
                            out+=0.00617284/10
                            this.millo=out.toFixed(8)
                        },100)
                    }
                }
            })
        },

        update(mid){
            let url='Upstate?mid='+mid;
            this.axios.get(url).then(result=>{})
        },

        ran2(max){
            //随机数
            return parseInt(Math.random()*max) 
        },
        can(){
            //背景样式
            var mat=document.getElementById("mat");
            for(var i=0;i<this.ran2(500)+100;i++){   
                var tag=document.createElement("div");
                tag.innerHTML=mat.firstElementChild.innerHTML;
                mat.appendChild(tag)
            }
            var tags=mat.children;
            for(var key of tags){
                key.style.left=`${this.ran2(100)}%`;
                key.style.top=`-${this.ran2(600)}%`;
            }
        }
    }    
}
</script>


<style>
    .loadimg{
        display: block;
        position: absolute;
        top:150px;
        left:50%;
        margin-left: -15%;
        width: 30%;
        animation: zuan 2s linear infinite;
    }
    @keyframes zuan {
        0%{transform: rotate(0)}
        100%{transform: rotate(360deg)}   
    }
    #mat{
        background: rgb(0,0,0) !important;
        width:100%;
        height:1366px;
    }
    #mat>div{
        display: flex;
        flex-direction: column;
        position: absolute;
        animation: down 2s linear infinite;
    }
    #mat span{
        font-size: 5px;
    }
    @keyframes down {
        0%{transform: translateY(700px)}

        100%{transform: translateY(3000px)}
    }
    #mat span:nth-child(1){
        color:rgba(19, 153, 59, 0.3);
    }
    #mat span:nth-child(2){
        color:rgba(19, 153, 59, 0.37);
    }
    #mat span:nth-child(3){
        color:rgba(19, 153, 59,0.44);
    }
    #mat span:nth-child(4){
        color:rgba(19, 153, 59, 0.51);
    }
    #mat span:nth-child(5){
        color:rgba(19, 153, 59, 0.58);
    }
    #mat span:nth-child(6){
        color:rgba(19, 153, 59, 0.65);
    }
    #mat span:nth-child(7){
        color:rgba(19, 153, 59, 0.72);
    }
    #mat span:nth-child(8){
        color:rgba(19, 153, 59, 0.79);
    }
    #mat span:nth-child(9){
        color:rgba(19, 153, 59, 0.86);
    }
    #mat span:nth-child(10){
        color:rgba(19, 153, 59, 0.93);
    }
    .production{
        position: absolute;
        top:70%;
        left: 10%;
    }
    .production>p{
        color: rgb(255,255,255);
        font-size: 16px;
    }
    .production>button{
        padding: 5px 50px;
        background:rgb(147, 212, 220); 
    }
</style>

