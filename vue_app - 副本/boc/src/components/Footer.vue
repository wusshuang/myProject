<template>
    <div>
        <nav class="mui-bar mui-bar-tab wid">
            <div class="mui-tab-item" @click="act('one')">
                <span class="mui-icon mui-icon-home"></span>
                <span class="mui-tab-label">首页</span>
            </div>
            <div class="mui-tab-item" @click="act('two')">
                <span class="mui-icon mui-icon-flag"></span>
                <span class="mui-tab-label">我的矿机</span>
            </div>
            <div class="mui-tab-item" @click="act('three')">
                <span class="mui-icon mui-icon-extra-trend"></span>
                <span class="mui-tab-label">交易大厅</span>
            </div>
            <div class="mui-tab-item" @click="act('four')">
                <span class="mui-icon mui-icon-contact"></span>
                <span class="mui-tab-label">个人中心</span>
            </div>
        </nav>
    </div>
</template>

<script>

export default {
    name:"Footer",
    data(){
        return {      
            one:this.$store.one,
            two:this.$store.two,
            three:this.$store.three,
            four:this.$store.four    
        }
    },
    methods:{
        act(aa){
            var path='';
            switch(aa){
                case 'one' : path='Home';
                break;
                case 'two' : path='Mill';
                break;
                case 'three' : path='Deal';
                break;
                case 'four' : path='Myhome';
                break;
                default : break;
            }
            this.$router.push(path)   
        }
    }
}
</script>

<style>
    .mui-bar{
        background: rgb(147, 212, 220) !important;
    }
    .mui-tab-label{
        font-size: 0.8rem !important;
        color:rgb(0,0,0);
        font-weight: 800;
    }
    .muiActive{
        color: rgb(0,0,255) !important;
    }
</style>


