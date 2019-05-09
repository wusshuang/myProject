<template>
    <div>
        <mt-header title="活动公告" fixed class="mt-header wid">
            <router-link to="Myhome" slot="left">
                <mt-button icon="back"></mt-button>
            </router-link>
        </mt-header>
        <div class="bullinfo">
            <h4>{{item.title}}:</h4>
            <hr>
            <p>{{item.one}}</p>
            <p>{{item.two}}</p>
            <p>{{item.three}}</p>
            <p>{{item.four}}</p>
            <p>{{item.five}}</p>
            <p>{{item.six}}</p>
            <p>{{item.seven}}</p>
        </div>
        <Footer></Footer>
    </div>
</template>

<script>
import Footer from "@/components/Footer"
export default {
    components: {
        Footer
    },
    data(){
        return {
            item:{},
            id:this.$route.query.tid
        }
    },
    created(){
        this.getBull()
    },
    methods:{
        getBull(){
            let url='Getbull'
            this.axios.get(url).then(result=>{
                if(this.id!=''){
                    for(let key of result.data){
                        if(key.tid==this.id){
                            this.item=key
                        }
                    }
                }else{
                    this.item=result.data[0];
                }
            })
        }
    }   
}
</script>

<style>
    .bullinfo{
        padding: 0 15px;
    }
    .bullinfo>h4{
        margin: 15px 0;
    }
    .bullinfo>p{
        color: black;
        margin: 20px 5px;
    }
</style>


