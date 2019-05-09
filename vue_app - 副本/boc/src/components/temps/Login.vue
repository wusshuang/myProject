<template>
    <div id="app-login">
        <img src="img/login4.png" class="img1">
        <div class="login-item">
            &nbsp;&nbsp;<p> 账 &nbsp;&nbsp;号：</p>
            <input type="text" name="phone" v-model="phone" placeholder="手机号登录" >
        </div>
        <div class="login-item">
            &nbsp;&nbsp;<p> 密 &nbsp;&nbsp;码：</p>
            <input type="password" name="logupwd" v-model="logupwd" placeholder="请输入密码">
        </div>
        <Sec ref="Child1"></Sec>
        <mt-button size="large" style="background:#93d4dc;color:#000" @click="login">登录</mt-button>            
    </div>
</template>

<script>

import {Toast} from "mint-ui"
import Sec from "@/components/temps/Sec"
export default {
    name:'Login',
    components:{
        Sec
    },
    data(){
        return {
            phone:"",
            logupwd:""
        }
    },
    methods:{
        login(){           
            if(!this.phone){
                Toast("账号不能为空！")
                return;
            }

            if(!this.logupwd){
                Toast("登录密码不能为空！")
                return;
            }

            if(this.$refs.Child1.sendMath()==1){
                let url="Login?";
                let postData=this.qs.stringify({
                    phone:this.phone,
                    logupwd:this.logupwd
                });
                this.axios.post(url,postData).then(result=>{
                    if(result.data.code==1){
                        if(result.data.data[0].state=="正常"){
                            //console.log(this.$store.state.uid);
                            if(this.$store.state.uid==result.data.data[0].uid){
                                Toast("该账号已经登录，请勿重复操作！")
                            }else{
                                Toast("登陆成功！");
                                sessionStorage.setItem('id',result.data.data[0].uid)
                                setTimeout(()=>{
                                    this.$router.push('Home')
                                },1000)
                            }                            
                        }else{
                            Toast("账号已被冻结！")
                        }
                    }else{
                        Toast("账号或密码错误!")
                    }
                })
            }else{
                Toast("验证码错误！");
            }
        }
    }
}
</script>

<style>
    
</style>

