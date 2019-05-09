<template>
    <div>
        <mt-header title="我的矿机" fixed class="mt-header wid"></mt-header>
        <table>
            <tr class="tr1">
                <td>编号</td>
                <td>类型</td>
                <td>租用时间</td>
                <td>已领收益</td>
                <td>状态</td>
                <td>操作</td>
            </tr>
            <tr v-for="(item,i) of mList" :key='i'>
                <td>{{item.mid}}</td>
                <td>{{item.type}}</td>
                <td>{{item.mtime | dataFilter}}</td>
                <td>{{item.yetget}}</td>
                <td>{{item.state}}</td>
                <td>
                    <button class="btn" @click="read" :data-id="item.mid">查看</button>
                </td>
            </tr>
        </table>
        <Footer></Footer>
    </div>
</template>

<script>

import Footer from "@/components/Footer"
export default {
    components:{
        Footer
    },
    data(){
        return {
            mList:[],
        }
    },
    created(){
        this.Mlist()
    },
    methods:{


        Mlist(){
            let url='Mlist';
            this.axios.get(url).then(result=>{
                if(result.data.code!=-1){
                    this.mList=result.data;
                }
            })
        },

        read(e){
            //打开矿机详情页面
            let mid=e.target.dataset.id;
            // console.log(mid)
            // Bus.$emit('send',this.mList[mid-1].mid)
            this.$store.commit('Mid',mid)
            // console.log(this.$store.state.mid)
            this.$router.push('Millpage')
        }
    }
}
</script>

<style>
    .btn{
        padding: 2px 3px !important;
        background: rgb(147, 212, 220);
    }
</style>
