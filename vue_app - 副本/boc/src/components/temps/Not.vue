<template>
    <div class='not'>
        <mt-header fixed class="mt-header wid">
            <router-link to="Myhome" slot="left">
                <mt-button icon="back"></mt-button>
            </router-link>
        </mt-header>
        <h1>! 404</h1>
        <h1>NOT FOUND !</h1>
        <h1>找不到请求资源 ！</h1>
    </div>
</template>

<style>
    .not{
        text-align: center
    }
    .not>h1{
        margin: 80px 0;
    }
</style>

