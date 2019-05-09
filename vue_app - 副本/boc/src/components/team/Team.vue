<template>
    <div>
        <mt-header title="我的团队" fixed class="mt-header wid">
            <router-link to="Myhome" slot="left">
                <mt-button icon="back"></mt-button>
            </router-link>
        </mt-header>
        <table>
            <tr class="tr1">
                <td>姓名</td>
                <td>编号</td>
                <td>等级</td>
                <td>状态</td>
                <td>团队人数</td>
                <td>注册时间</td>
            </tr>
            <tr v-for="(item,i) of teamList" :key="i">
                <td>{{item.uname}}</td>
                <td>{{item.phone}}</td>
                <td>{{item.rank}}</td>
                <td>{{item.state}}</td>
                <td>{{item.tteam}}</td>
                <td>{{item.rtime | dataFilter}}</td>
            </tr>
        </table>
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
            teamList:[]
        }
    },
    created(){
        this.team()
    },
    methods:{
        team(){
            let url='GetTeam';
            this.axios.get(url).then(result=>{
                this.teamList=result.data;
            })
        }
    }
}
</script>