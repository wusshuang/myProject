<template>
    <div>
        <input type="text" name="title" v-model="title">
        <input type='text' name='one' v-model="one">
        <input type='text' name='two' v-model="two">
        <input type='text' name='three' v-model="three">
        <input type='text' name='four' v-model="four">
        <input type='text' name='five' v-model="five">
        <input type='text' name='six' v-model="six">
        <input type='text' name='seven' v-model="seven">
        <button @click='bullText'>提交</button>    
    </div>
</template>

<script>

import {Toast} from "mint-ui"
export default {
    data(){
        return {
            title:"",
            one:"",
            two:"",
            three:"",
            four:"",
            five:"",
            six:"",
            seven:""
        }
    },
    methods:{
        bullText(){
            if(this.title==''){
                Toast('不能为空！')
                return
            }else if(this.one==''){
                Toast('不能为空！')
                return
            }

            let url=`Input?`;
            let postData=this.qs.stringify({
                title:this.title,
                one:this.one,
                two:this.two,
                three:this.three,
                four:this.four,
                five:this.five,
                six:this.six,
                seven:this.seven
            })
            this.axios.post(url,postData).then(result=>{
                if(result.data.code==1){
                    Toast(result.data.msg)
                    this.title='';
                    this.one='';
                    this.two='';
                    this.three='';
                    this.four='';
                    this.five='';
                    this.six='';
                    this.seven=''
                }else{
                    Toast('木发出去，再来一次！')
                }
            })
        }
    }
}
</script>

