<template>
    <div>
        <mt-header title="个人中心" fixed class="mt-header wid"></mt-header>
        <div class="header">
            <img src="img/logo4.png" alt="" class='logoimg'>
            <ul>
                <li>用户编号：{{uInfo.phone}}</li>
                <li>用户等级：{{uInfo.rank}}</li>
                <li>团队人数：{{tList}}</li>
                <li>当前算力：{{suanLi}} G</li>
            </ul>     
        </div> 
        <ul class="list">
            <li>
                <p>{{uInfo.deposit}}</p>
                <p>账户余额</p>
            </li>
            <li>
                <p>{{mInfo.length}}台</p>
                <p>有效矿机</p>
            </li>
            <li>
                <p>{{Math.floor(uInfo.freeze*100)/100}}</p>
                <p>冻结资产</p>
            </li>
            <li>
                <p>{{Moutput}}</p>
                <p>矿机收益</p>
            </li>
            <li>
                <p>{{uInfo.rincome}}</p>
                <p>推荐收益</p>
            </li>
            <li>
                <p>{{Moutput+Number(uInfo.rincome)}}</p>
                <p>累计收益</p>
            </li>
        </ul>
        <ul class="mui-table-view mui-grid-view mui-grid-9">
		    <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3" v-for="(item,i) of gridlist" :key="i">
                <a href="#">
                    <img :src="'img/'+item.img_url"  @click="jump" :data-id="item.id">
                    <div class="mui-media-body">{{item.title}}</div>
                </a>
            </li>
        </ul>
        <mt-button size="large" type="primary" style="background:#aae2de;color:#000;margin:10px 0;" @click="quit">退出登录</mt-button>
        <Footer class="wid"></Footer>
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
            gridlist:[
                {id:1,title:"我的团队",img_url:"menu2.png"},
                {id:2,title:"财务明细",img_url:"menu1.png"},
                {id:3,title:"实名认证",img_url:"menu3.png"},
                {id:4,title:"订单记录",img_url:"menu9.png"},
                {id:5,title:"活动公告",img_url:"menu5.png"},
                {id:6,title:"推广中心",img_url:"menu4.png"},
                {id:7,title:"联系客服",img_url:"menu7.png"},
                {id:8,title:"关于我们",img_url:"menu8.png"},
                {id:9,title:"帮助中心",img_url:"menu6.png"}
            ],
            uInfo:{},
            mInfo:[],
            tList:'',
            suanLi:0, //算力
            Moutput:0 //已领收益
        }
    },
    created(){
        this.UserInfo() //获取用户信息列表
        this.MillInfo() //获取用户矿机信息
        this.Team() //获取用户团队
    },
    methods:{



        //     this.$store.commit('newNum',16)


        UserInfo(){
            let url='Self';
            this.axios.get(url).then(result=>{
                this.uInfo=result.data[0]
                this.uInfo.deposit=Math.floor(result.data[0].deposit*10000)/10000;
                this.$store.commit('deposit',this.uInfo.deposit);
            })     
        },

        MillInfo(){
            let url='Mlist';
            this.axios.get(url).then(result=>{
               if(result.data.code!=-1){
                    this.MillInfoo(result.data)
               }
            })
        },
        MillInfoo(arr){
                let suan=0;
                let sum=0;
                
                let rank1=0;//判断用户等级
                let rank2=0;
                let rank3=0;
                let url1='';
                for(var key of arr){
                    if(key.state=='运行中'){
                        switch(key.type){
                            case '小型云矿机' : suan=20
                            break
                            case '中型云矿机' : suan=200
                            break
                            case '大型云矿机' : suan=2000
                            break
                            default : suan=2
                            break
                        }
                        this.suanLi+=suan
                        this.mInfo.push(key)
                    }

                    //修改用户等级     
                    if(key.type=='大型云矿机'){
                        rank1=4
                    }else if(key.type=='中型云矿机'){
                        rank2=3
                    }else if(key.type=='中型云矿机'){
                        rank3=2
                    }

                    sum+=Number(key.yetget)
                }

                if(rank1==4){
                    url1='UpdateRank?rank=四级矿工';
                }else if(rank2==3){
                    url1='UpdateRank?rank=三级矿工';
                }else if(rank3==2){
                    url1='UpdateRank?rank=二级矿工';
                }

                if(url1!=''){
                    this.axios.get(url1).then(result=>{})
                }
                this.Moutput=Math.floor(sum*10000)/10000
        },
        
        Team(){
            let url='GetTeam';
            this.axios.get(url).then(result=>{
                this.Teamm(result.data)
            })
        },
        Teamm(arr){
            let tcount=0;
            for(let key of arr){
                tcount+=Number(key.tteam)
            }
            tcount=tcount+arr.length;
            this.tList=tcount;
            let url='GetTeamCount';
            let postData=this.qs.stringify({
                tteam:tcount
            })
            this.axios.post(url,postData).then(result=>{})
        },

        quit(){
            // this.$store.commit('newNum',false)
            window.sessionStorage.removeItem('id')
            // console.log(window.sessionStorage.getItem);
            this.$router.push('/')
        },

        jump(e){
            var id=e.target.dataset.id;
            let path="";

            if(id==3){
                if(this.uInfo.cardurl){
                    Toast('您已上传实名照片，请勿重复操作！')
                    return
                }
            }

            switch(id){
                case '1' : path="Team"
                break
                case '2' : path="Self";
                break
                case '3' : path="Upload";
                break
                case '4' : path="Deallist";
                break
                case '5' : path="Bull";
                break
                case '6' : path="Referrer";
                break
                case '7' : path="Call";
                break
                case '8' : path="About";
                break
                case '9' : path="Help";
                break
            }
            this.$router.push(path);
        }
    }
}
</script>

<style>
    .header{
        background: rgb(147, 212, 220);
        width: 100%;
        display: flex;
    }
    .logoimg{
        width: 40%;
        height: 40%;
        padding-top: 10px;
        /* animation:zhuan 2s; */
    }
    /* @keyframes zhuan{
        0%{transform: translateX(0)};
        100%{transform: translateX(100px)}
    } */
    .header>ul{
        border-left: 1px solid rgb(0,0,0);
        padding-left: 20px;
        width: 60%;
        list-style: none;
    }
    .header>ul>li{
        color: rgb(0,0,0);
        font-size: 13px;
    }
    .list{
        list-style: none;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-around;
        width: 100%;
        padding-left: 0;
        margin: 5px 0;
    }
    .list>li{
        width: 33%;
        text-align: center;
        background: rgb(170,226,222);
        margin:5px 0;
    }
    .list>li>p{
        color: rgb(0,0,0);
        font-weight: 900;
        font-size: 16px;
    }

    .mui-table-view{
        background: rgb(255,255,255) !important;
    }
    .mui-table-view img{
        width: 70%;
    }
    .mui-media-body{
        font-weight: 700;
    }
</style>
