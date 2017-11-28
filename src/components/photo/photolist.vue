<template lang="html">
    <div class="temp">
        <!-- 图片分类 -->
        <div class="cate">
            <ul v-bind="{style : 'width:' + ulWidth + 'px'}">
                <li>全部</li>
                <li v-for="item in cates" @click="getimages(item.id)">{{item.title}}</li>
            </ul>
        </div>

        <!--2.0 图片列表-->
        <div class="imglist">
            <ul>
                <li v-for="item in list">
                    <router-link v-bind="{to:'/photo/photoinfo/'+item.id}">
                        <img v-lazy="item.img_url">
                        <div class="desc">
                            <h5 v-text="item.title"></h5>
                            <p v-text="item.zhaiyao"></p>
                        </div>
                    </router-link>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import { Toast } from 'mint-ui';
import common from '../../kits/common.js';
export default {
    data(){
        return {
            // 用来存储图片分类数据的数据
            cates : [],
            ulWidth : 320,
            list : [
                // {
                //     "id": 52,
                //     "title": "柳岩出席设计师好友兰玉的高级成衣发布Show",
                //     "img_url": "http://182.254.146.100:8080/upload/201504/18/thumb_201504181426524427.jpg",
                //     "zhaiyao": "北京四季酒店,柳岩出席设计师好友兰玉的高级成衣发布Show,她身穿兰玉设计的纯白卡肩礼服惊艳登场,优雅晚装发髻搭配翡翠镶钻珠宝,举手投足尽显大气温婉,而卡肩低胸的礼服设计更是衬托出柳岩傲人的事业线资本,性感指数爆灯,入场即引来现场阵阵骚动,柳岩轻松看秀全程甜笑连连,心情…"
                //     }
            ]
        }
    },
    created(){
        this.getcates();
        var all = 0;
        this.getimages(all);
    },
    methods : {
        getcates(){
            var url = common.apidomain + '/api/getimgcategory';
            this.$http.get(url).then(function(res){
                var data = res.body;
                if (data.status != 0) {
                    Tocast(data.message);
                    return;
                }
                // 数据赋值操作
                this.cates = data.message;

                // 给ul动态计算宽度
                var w = 69;
                var count = res.body.message.length + 1;
                this.ulWidth = w * count;
            })
        },

        getimages(cateid){
            cateid = cateid || 0;
            var url = common.apidomain + '/api/getimages/' + cateid;

            this.$http.get(url).then(function(res){
                var body = res.body;
                if (body.status !=0 ) {
                    Toast(res.body.message);
                    return;
                }
                this.list = body.message;
            })

        }
    }
}
</script>

<style lang="css">
/*1.0 图片分类*/
.cate{
    width: 375px;
    height: 50px;
    max-width: 375px;
    overflow-x: auto;
}
.cate ul{
    /*width: 1000px;*/
    margin: 0px;
    padding-left: 10px;
}
.cate li{
    list-style: none;
    display: inline-block;
    color:#0094ff;
    font-size: 14px;
    padding-left: 6px;
}
.cate li + li{
    border-left: 1px solid #ccc;
    padding-right: 6px;
}
/*实现图片列表样式*/
.imglist{

}
.imglist ul {
    padding-left: 0px;
}
.imglist li{
    list-style:none;
    position: relative ;
}
.imglist img{
    width:100%;
    height: 300px;
}

.desc{
    width: 100%;
    background-color: rgba(0,0,0,0.2);
    position: absolute;
    bottom: 2px;
    left: 0px;
}

.desc h5{
    color: #ffffff;
    font-weight: bold;
}
.desc p{
    color:#fff;
}

image[lazy=loading] {
    width: 40px;
    height: 300px;
    margin: auto;
}
</style>
