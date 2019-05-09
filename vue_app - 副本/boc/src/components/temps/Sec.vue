<template>
    <div>
        <div class="login-item">
            &nbsp;&nbsp;<p>验证码：</p>
            <input type="text" name="security" v-model="security" class="login-input1">
            <span @click="ma" class="sec">
                {{math}}
                <span class="m"></span>
                <span class="m"></span>
                <span class="m"></span>
                <span class="m"></span>
                <span class="m"></span>
                <span class="m"></span>
            </span>
        </div>
    </div>
</template>

<script>

export default {
    name:'Sec',
    data(){
        return {
            security:"",
            math:""
        }
    },
    mounted(){
        this.ma()
    },
    methods:{
        sendMath(){

            if(this.security==this.math){
                return 1;
            }else{
                setTimeout(()=>{
                    this.ma()
                },2000);
                return 0;
            }
        },
        ran1(max){
            return parseInt(Math.random()*max)
        },
        ma(){
            var sec=document.querySelector(".sec");
            this.math=this.ran1(10)+''+this.ran1(10)+''+this.ran1(10)+''+this.ran1(10);
            sec.style.background=`rgba(${this.ran1(255)},${this.ran1(255)},${this.ran1(255)},0.3)`;
            sec.style.color=`rgba(${this.ran1(255)},${this.ran1(255)},${this.ran1(255)},0.6)`;
            var bs=document.querySelectorAll(".m");
            for(var b of bs){
                b.style.transform=`rotate(${this.ran1(360)}deg)`;
                b.style.top=`${this.ran1(80)}%`;
                b.style.left=`${this.ran1(50)}%`;
                b.style.background=`rgb(${this.ran1(255)},${this.ran1(255)},${this.ran1(255)})`;
            }
        }
    }
}
</script>

<style>
    .login-input1{
        width: 60% !important;
    }
   .sec{
        display:inline-block;
        width: 40%;
        height: 40px;
        border: 1px solid rgb(204,204,204);
        line-height: 40px;
        text-align: center;
        font-weight: 800;
        font-size: 20px;
        letter-spacing: 12px;
        border-left: 0;
        position: relative;
        overflow: hidden;
    } 
    .sec>span{
        position: absolute;
        display: inline-block;
        width: 70px;
        height:1px; 
    }

</style>


