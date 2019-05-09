<template>
    <table>
        <mt-header title="联系客服" fixed class="mt-header wid">
            <router-link to="Myhome" slot="left">
                <mt-button icon="back"></mt-button>
            </router-link>
        </mt-header>
        <tr v-for="(item,i) of caller" :key="i">
            <td>
                <h4>{{item.id}}号客服：</h4>
            </td>
            <td>
                <img :src="'img/'+item.img_url" alt="">
            </td>
        </tr>
    </table>
</template>

<script>
export default {
    data(){
        return {
            caller:[
                {id:1,img_url:"erweima.png"},
                {id:2,img_url:"erweima.png"},
                {id:3,img_url:"erweima.png"},
                {id:4,img_url:"erweima.png"}
            ]
        }
    }
}
</script>

