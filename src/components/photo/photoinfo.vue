<template lang="html">
    <div class="temp">
        <div class="desc">
            <div class="title">
                <h4>{{photoinfo.title}}</h4>
                <p>
                    <span>{{photoinfo.add_time | datefmt('YYYY-MM-DD HH:mm:ss')}}</span>
                    <span>{{photoinfo.click}}次浏览</span>
                </p>
                <p class="line"></p>
            </div>

        </div>

        <!--缩略图-->
        <div class="mui-content">
            <ul class="mui-table-view mui-grid-view mui-grid-9">
                <li :key="item.id" v-for="(item, index) in list" class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
                    <img class="preview-img"  :src="item.src" height="100" @click="$preview.open(index, list)">
                </li>
            </ul>
            <!--图片详情- 摘要部分-->
			<p v-html="photoinfo.content"></p>
        </div>

        <!--2.0。3 集成评论组件-->
        <div id="comment">
            <comment :id="id"></comment>
        </div>
    </div>
</template>

<script>
import comment from '../subcom/comment.vue';
import common from '../../kits/common.js';
import { Toast } from 'mint-ui';
// console.log(this);
// this.$preview.open(index, list)
export default {
    components:{
        comment //2.0.1 注册评论组件
    },
    data(){
        return {
            id : 0,
            photoinfo : {},
            list :[]
        }
    },
    created(){
        this.id = this.$route.params.id;
        this.getinfo();
        this.getImgs();

    },
    methods:{
        getinfo(){
            var url = common.apidomain + "/api/getimageInfo/" + this.id;

            this.$http.get(url).then(function(res){
                var body = res.body;
                if (body.status != 0) {
                    Toast(body.message);
                    return;
                }
                this.photoinfo = body.message[0];
            })
        },
        getImgs(){
            var url = common.apidomain + "/api/getthumimages/" + this.id;

            this.$http.get(url).then(function(res){
                var body = res.body;
                if (body.status != 0) {
                    Toast(body.message);
                    return;
                }
                body.message.forEach((item) => {
                    var img = new Image();
                    img.src = item.src;

                    img.onload = function(){
                        item.h = img.height;
                        item.w = img.width;
                    }
                });

                this.list = body.message;
            })
        }
    }
}
</script>

<style lang="css">
/*图片详情样式*/
.desc{
    padding: 10px;
    position: static;
}
.desc  .title h4{
    color: #0094ff;
}
.desc .title p{
    color:rgba(0,0,0,0.4);
    margin-top: 10px;
}

.desc .title .line{
    width: 100%;
    height: 1px;
    border-bottom: 1px solid rgba(0,0,0,0.4);
}

.mui-content,.mui-content ul{
    background-color: #fff;
}
.mui-grid-view.mui-grid-9 .mui-table-view-cell{
    border-right:0px;
    border-bottom:0px;
}
.mui-grid-view.mui-grid-9{
    border-top:0px;
    border-left:0px;
}

/* 9宫格缩略图的样式*/
.mui-content img{
    width: 100px;
    height: 100px;
}
</style>
