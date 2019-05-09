<template>
    <div>
        <Login v-show="this.$store.state.Login"></Login>
        <Reg v-show="this.$store.state.Reg" :sendChild='boss'></Reg>
        <br>
        <br>
        &nbsp;&nbsp;<span @click="change">{{chang}}</span>
        <br>
        <br>
        <router-link to="#" class="load">点击下载APP</router-link>   
    </div>
</template>

<script>

import Login from "@/components/temps/Login"
import Reg from "@/components/temps/Reg"
export default {
    components:{
        Login,
        Reg
    },
    data(){
        return {
            chang:'注册账号',
            login:{
                one:true,
                two:false
            },
            boss:''
        }
    },
    created(){
        if(this.$route.query.boss){
            this.$store.commit('login',false)
            this.$store.commit('reg',true)
            this.boss=this.$route.query.boss
        }
    },
    methods:{
        change(){
            if(this.$store.state.Login){
                this.$store.commit('login',false)
                this.$store.commit('reg',true)
                this.chang='登录'
                // console.log(this.$store.state.Login)
                // console.log(this.$store.state.Reg)
            }else{
                this.$store.commit('login',true)
                this.$store.commit('reg',false)
                this.chang='注册账号'        
            }
        }
    }
}
</script>

