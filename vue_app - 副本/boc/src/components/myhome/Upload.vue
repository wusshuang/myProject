
<template>
    <div>
        <mt-header title="实名认证" fixed class="mt-header wid">
            <router-link to="Myhome" slot="left">
                <mt-button icon="back"></mt-button>
            </router-link>
        </mt-header>

        <div class="ab-tit">照片示例：</div>
        <div class="card">
            <img src="img/card1.png" alt="">
        </div>
        <hr>
        <!-- <img :src="'upload/'+imgurl" alt=""> -->
        <hr>
        <div class="waring">
            &nbsp;<h4>注意:</h4>
            <p>&nbsp;&nbsp;上传身份证正面照：字迹必须清晰，请严格按照示例照片上传，如果证件照片信息有误，封号。</p>
        </div>
        <hr>
        <!-- <form action="uploadFile?did=0"  method="POST" enctype="multipart/form-data" @submit='uploadimg'>
            <input type="file" name="mypic" class="upload" id='img'>
            <br>
            <br>
            <input type="submit" value="上传照片" class="submit">
        </form> -->

        <div>
            <input  accept="image/*" type="file" name="mypic" class="upload" id="image">
            <br>
            <input type="button" value="上传照片" class="submit" @click="uploadimg">
        </div>
    </div>
</template>
<script>

import {Toast} from 'mint-ui'
export default {
    data(){ return { imgurl:"" } },
    methods:{
        uploadimg(){
            var file = document.getElementById("image").files[0];
            if(file===undefined){
                Toast('请选择要上传的图片！')
                return
            }
            var formdata1=new FormData();// 创建form对象
            formdata1.append('mypic',file,file.name);// 通过append向form对象添加数据,可以通过append继续添加数据
            //或formdata1.append('img',file);
            let config = {
                headers:{'Content-Type':'multipart/form-data'}
            };  //添加请求头
            this.axios.post('uploadFile?did=0',formdata1,config).then(result=>{   //这里的/xapi/upimage为接口
                if(result.data.code==1){
                    // this.imgurl=result.data;
                    Toast('上传成功！')
                    setTimeout(()=>{
                        this.$router.push('Myhome')
                    },1500)
                }
                // console.log(result.data)
            })

        }
    }
}
</script>


<style>
    form{
        padding: 0 10px;
    }
    .submit{
        margin-left: 70%;
    }
    .card>img{
        width: 80%;
        display: block;
        margin: 0 auto;
    }
    .waring{
        padding:0 10px;
    }
    .waring>p{
        color: rgb(165,42,42)
    }
</style>





