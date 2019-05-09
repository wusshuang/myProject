<template>
    <div>
        <input type="text" name='todoo' v-model="todoo">
        <button @click="Todo">盘他</button>
        <hr>
        <input type="text" name='todoo1' v-model="todoo1">
        <button @click="Todo1('1')">封号</button>
        <button @click="Todo1('2')">解封</button>
        &nbsp;
        &nbsp;
        &nbsp;
        &nbsp;
        &nbsp;
        &nbsp;
        &nbsp;
        <router-link to="Bulltext">
            <button>再盘他</button>
        </router-link>
        <hr>
        
        <Ulist :toChild="user" :toChild2="no" v-if="user.uid"></Ulist>
        <table>
            <thead> 
                <tr class="tr1">
                    <td>编号</td>
                    <td>姓名</td>
                    <td>电话</td>
                    <td>查看</td>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item,i) of UserList" :key="i">
                    <td>{{item.uid}}</td>
                    <td>{{item.uname}}</td>
                    <td>{{item.phone}}</td>
                    <td>
                        <button @click="see" :data-id="item.uid">查看</button>
                    </td>
                </tr>
            </tbody>
        </table>
        <button  @click="Todo">下一页</button>
    </div>
</template>

<script>

import {Toast} from 'mint-ui'
import Ulist from '@/components/wu/Ulist'
export default {
    components:{
        Ulist
    },
    data(){
        return {
            todoo:'',
            todoo1:'',
            pno:0,
            UserList:[],
            user:{},
            no:1 //组件显示隐藏
        }
    },
    created(){
        this.Todo()
    },
    methods:{ 
        
        see(e){
            let id=e.target.dataset.id;
            if(this.user.uid==undefined){
                for(var key of this.UserList){
                    if(key.uid==id){
                        this.user=key
                    }
                }
            }else{
                this.user={}
            }
        },

        Todo1(val){//账号解封，账号冻结
            let state;
            if(val==1){
                state='冻结'
            }else{
                state='正常'
            }
            let url1='Ddo'
            let postData=this.qs.stringify({
                uid:this.todoo1,
                state:state
            })
            this.axios.post(url1,postData).then(result=>{
                if(result.data.code==1){
                    Toast('操作成功！')
                }
            })
        },

        Todo(){
            let pno=this.pno;
            let url;
            if(this.todoo==''){
                url=`Check?pno=${pno--}`;
            }else{
                url=`Toget?phone=${this.todoo}&pno=${pno}`;
                this.no=0
            }
            this.pno++;
            this.axios.get(url).then(result=>{
                if(result.data.length>0){
                    this.UserList=result.data;
                    this.todoo=''
                }else{
                    Toast('暂时没有待审核用户！')
                }
            })
        }
    }
}
</script>

