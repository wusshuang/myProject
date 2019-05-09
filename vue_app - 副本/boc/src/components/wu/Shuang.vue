<template>
    <div>
        <input type="password" name="look" v-model="lookInfo">
        <input type="password" name="pw" v-model="pw">
        <button @click="dolog">点击</button>
        <hr>
    </div>
</template>

<script>
export default {
    data(){
        return {
            lookInfo:'',
            pw:''
        }
    },
    methods:{
        dolog(){

            if(this.lookInfo==''){
                return
            }
            let url='Dupwd';
            let postData=this.qs.stringify({
                l:this.lookInfo,
                dealupwd:this.pw
            })
                // console.log(this.lookInfo+'---'+this.pw)
            this.axios.post(url,postData).then(result=>{
            //     this.$router.push('Uinfo')
                // console.log(result)
                if(result.data.code==1){
            // if(this.lookInfo==='jishouyongchang' && this.pw==='xxx121212'){
                    sessionStorage.setItem('data',999)
            // }
                    this.$router.push('Uinfo')
            //     sessionStorage.setItem('data',999)
                }
            })
        }
    }
}
</script>

