<template>
    <div>
        <mt-header title="新用户注册" fixed class="mt-header wid"></mt-header>
        <div class="reg">
            <div class="login-item">
                &nbsp;&nbsp;<p> 手机号：</p>
                <input type="text" name="phone" v-model="phone" placeholder="请输入正确的手机号" @blur="searchPhone">
            </div>
            <div class="login-item">
                &nbsp;&nbsp;<p> 姓 &nbsp;&nbsp;名：</p>
                <input type="text" name="uname" v-model="uname" placeholder="请输入本人真实姓名">
            </div>
            <div class="login-item">
                &nbsp;&nbsp;<p> 支付宝：</p>
                <input type="text" name="pay" v-model="pay" placeholder="请输入本人支付宝交易账号">
            </div>
            <div class="login-item">
                &nbsp;&nbsp;<p> 登录密码：</p>
                <input type="password" name="logupwd1" v-model="logupwd1" placeholder="请输入6~12位字母加数字登录密码">
            </div>
            <div class="login-item">
                &nbsp;&nbsp;<p> 确认密码：</p>
                <input type="password" name="logupwd2" v-model="logupwd2" placeholder="请输入确认登录密码">
            </div>
            <div class="login-item">
                &nbsp;&nbsp;<p> 交易密码：</p>
                <input type="password" name="dealupwd1" v-model="dealupwd1" placeholder="请输入6位数字交易密码">
            </div>
            <div class="login-item">
                &nbsp;&nbsp;<p> 确认密码：</p>
                <input type="password" name="dealupwd2" v-model="dealupwd2" placeholder="请输入确认交易密码">
            </div>
            <div class="login-item">
                &nbsp;&nbsp;<p> 推荐人 ：</p>
                <input type="text" name="boss" v-model="boss" placeholder="请输入推荐人电话">
            </div>
            <Sec ref="Child"></Sec>
        </div>
        <hr>
        <div class="agree">
            <h4>请认真阅读以下注意事项：</h4>
            <br>
            ①<span>
                 注册年龄必须在18~70岁之间，具备独立自主能力，同一身份证只能注册一个账号，收款资料必须真实有效，注册成功以后不可修改，手机号、姓名、支付宝必须绝对统一，否则冻结账号，无法完成交易。
            </span>
            <br>
            <br>
            ②<span>
                 交易规则需按平台规章制度，买卖匹配成功必须一小时内完成付款、确认收款，如有违规冻结账号。
            </span>
            <br>
            <br>
            ③<span>
                 区块链被誉为财富第九波，本平台完全去中心化，矿工点对点交易，所有资金不经过平台，无众筹充值提现端口，完全免费注册随进随出，投资自由，风险自控，请用闲散资金参与。
            </span>
            <br>
            <br>
            <div>
                <input type="checkbox" v-model="checked">
                <span class="check"> 我已认真阅读上述规则，同意加入BOC旷工联盟</span>
            </div>
        </div>
        <hr>
        <br>
        <div>
            <mt-button size="large" style="background:#93d4dc;color:#000" @click="reg">注册</mt-button>
        </div>
    </div>
</template>

<script>

import {Toast} from "mint-ui"
import Sec from "@/components/temps/Sec"
export default {
    name:'Reg',
    components:{
        Sec
    },
    props:['sendChild'],
    data(){
        return {
            phone:"",
            uname:"",
            pay:"",
            logupwd1:"",
            logupwd2:"",
            dealupwd1:"",
            dealupwd2:"",
            boss:'',
            security:'',
            math:'',
            boss:'',
            checked:""
            
            // this.$route.query.boss
        }
    },
    created(){
        if(this.sendChild){
            this.boss=this.sendChild
        }
    },
    methods:{

        searchPhone(){
            let url="Searchphone?phone="+this.phone;
            this.axios.get(url).then(result=>{
                if(result.data.code==1){
                    Toast("该号码已经存在!");
                    this.phone=""
                }
            })
        },

        reg(){
            
            let reg1=/^1[3-8][0-9]{9}$/;
            if(!reg1.test(this.phone)){
                Toast("手机号格式不正确！");
                return;
            }

            let reg2=/^[\u4E00-\u9FA5]{2,4}$/;
            if(!reg2.test(this.uname)){
                Toast("姓名格式不正确,请输入2~4位汉字。");
                return;
            }

            let reg3=/^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{6,12}$/;
            if(!reg3.test(this.logupwd1)){
                Toast("登录密码格式不正确，请重新输入")
                return;
            }
            if(this.logupwd2!=this.logupwd1){
                Toast("登录密码不一致，请重新输入");
                return;
            }

            let reg4=/^[0-9]{6}$/;
            if(!reg4.test(this.dealupwd1)){
                Toast("交易密码格式不正确，请重新输入");
                return;
            }
            if(this.dealupwd1!=this.dealupwd1){
                Toast("交易密码不一致，请重新输入");
                return;
            }

            if(this.phone==this.boss){
                Toast('推荐人不能填自己！');
                return
            }

            if(this.boss==''){
                Toast('推荐人不能为空！');
                return
            }

            if(this.security!=this.math){
                Toast("验证码不正确！");
                return;
            }

            if(!this.checked){
                Toast("请认真阅读注意事项");
                return
            }

            let postData=this.qs.stringify({
                phone:this.phone,
                uname:this.uname,
                pay:this.pay,
                logupwd:this.logupwd1,
                dealupwd:this.dealupwd1,
                boss:this.boss
            });
            
            if(this.$refs.Child.sendMath()==1){
                let url="Reg?"
                this.axios.post(url,postData).then(result=>{
                    if(result.data.code==1){
                        Toast("注册成功");
                        setTimeout(()=>{
                            this.$store.commit('login',true)
                            this.$store.commit('reg',false)
                        },2000)
                    }
                })
            }else{
                Toast("验证码错误！")
            }
        }
    }
}
</script>

<style>
    .reg{
        margin-top: 20px;
    }
  
    .load{
        display:block !important;
        margin-left:35% !important;
    }
    .agree{
        padding: 5px;
    }
    .agree span{
        color: rgb(75,75,75);
        font-size: 12px;
    }
    .check{
        color: rgb(255,0,0) !important;
        font-weight: 700;
        font-size: 14px !important;
    }
</style>


