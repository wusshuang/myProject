<template>
    <div>   
        <mt-header title="活动公告" fixed class="mt-header wid">
            <router-link to="Myhome" slot="left">
                <mt-button icon="back"></mt-button>
            </router-link>
        </mt-header>
        <div class="bull" v-for="(item,i) of text" :key="i">
            <div class="bulltitle" @click="open" :data-id="item.tid">
                <span>{{item.tid}}.</span>
                <span>{{item.title}}...</span>
                <span>{{item.ttime | dataFilter}}</span>
                <span>▼</span>
            </div>
            <!-- <p class="bulltext" v-if="item.tid==o">&nbsp;&nbsp;&nbsp;{{item.text}}</p> -->
            <hr>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return {
            text:[]
        }
    },
    created(){
        this.tload()
    },
    methods:{
        open(e){
            let o=e.target.parentElement.dataset.id;
            this.$router.push(`BullInfo?tid=${o}`)
        },
        tload(){
            let url='Getbull'
            this.axios.get(url).then(result=>{
                // let c=this.text.concat(result.data);
                this.text=result.data;
            })
        }
    }
}
</script>


<style>
    .bull{
        padding: 0 6px;
        margin-top: 10px;
    }
    .bulltitle{
        display: flex;
        justify-content: space-between;
    }
    .bulltitle>span:nth-child(2){
        width: 60%;
        font-size: 16px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
    }
    .bulltext{
        padding:15px 20px ;
        font-size: 14px;
        color: rgb(165,42,42);
        line-height: 40px
    }
</style>

