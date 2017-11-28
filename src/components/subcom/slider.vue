<template lang="html">
    <div class="temp">
        <!-- 1.0 轮播图：mint-ui中的 swipe组件实现 -->
        <mt-swipe :auto="4000">
            <!-- <mt-swipe-item>1</mt-swipe-item>
            <mt-swipe-item>2</mt-swipe-item> -->
            <mt-swipe-item v-for="item in imgs" :key="item.id">
                <img v-bind:src="item.img">
            </mt-swipe-item>

        </mt-swipe>
    </div>
</template>

<script>
    export default {
        // 用来接收父组件传递过来的数据
        props : ['imgs']
    }
</script>

<style lang="css" scoped>
    .mint-swipe{
        height: 300px;
    }
    .mint-swipe-item img{
        width: 100%;
        height: 100%
    }
    .mint-swipe-item{
        background-color: red;
        width: 100%;
        height: 300px;
    }
</style>
