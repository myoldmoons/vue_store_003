<template lang="html">
    <div class="temp">
        <!-- 轮播图组件 -->
        <!-- <mt-swipe :auto="4000">
            <mt-swipe-item v-for="item in list" :key="item.id">
                <img v-bind:src="item.img">
            </mt-swipe-item>
        </mt-swipe> -->
        <slider :imgs="list"></slider>

        <!-- 九宫格 -->
        <div class="mui-content">
		        <ul class="mui-table-view mui-grid-view mui-grid-9">
		            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
                        <router-link to="/news/newslist">
		                    <span class="mui-icon mui-icon-home"></span>
		                    <div class="mui-media-body">新闻资讯</div>
                        </router-link>
                    </li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
                        <router-link to="/photo/photolist">
		                    <span class="mui-icon mui-icon-email"></span>
		                    <div class="mui-media-body">图片分享</div>
                        </router-link>
                    </li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
                        <router-link to="/goods/goodslist">
		                    <span class="mui-icon mui-icon-chatbubble"></span>
		                    <div class="mui-media-body">商品购买</div>
                        </router-link>
                    </li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
                        <router-link to="/feedback">
		                    <span class="mui-icon mui-icon-location"></span>
		                    <div class="mui-media-body">留言反馈</div>
                        </router-link>
                    </li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
                        <router-link to="/video">
		                    <span class="mui-icon mui-icon-search"></span>
		                    <div class="mui-media-body">视频专区</div>
                        </router-link>
                    </li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
                        <router-link to="/callme">
		                    <span class="mui-icon mui-icon-phone"></span>
		                    <div class="mui-media-body">联系我们</div>
                        </router-link>
                    </li>
		        </ul>
		</div>
    </div>
</template>

<script>
import { Toast } from 'mint-ui';
import common from '../kits/common.js';
import slider from './subcom/slider.vue';
export default {
    components : {
        slider
    },
    data(){
        return {
            list : [
                // {
                //     "url": "http://www.itcast.cn/subject/phoneweb/index.html",
                //     "img": "http://8867345.s21i-8.faiusr.com/2/ABUIABACGAAg_dj2wwUowvbjywUwgAU4wAI!640x640.jpg"
                // },
                // {
                //     "url": "http://www.itcast.cn/subject/phoneweb/index.html",
                //     "img": "http://8867345.s21i-8.faiusr.com/2/ABUIABACGAAgmMv3wwUopMqcATCABTjAAg!640x640.jpg"
                // },
                // {
                //     "url": "http://www.itcast.cn/subject/phoneweb/index.html",
                //     "img": "http://8867345.s21i-8.faiusr.com/2/ABUIABACGAAgyr6AxAUo2IHXtQMwgAU4wAI!640x640.jpg"
                // }
            ]
        }
    },
    created : function(){
        // 当页面中的data和methods对象都创建完毕之后，就会自动调用created方法
        this.getImgs()
    },
    methods : {
        getImgs(){
            // 实现轮播组件中的数据请求
            var url = common.apidomain + '/api/getlunbo';
            this.$http.get(url).then(function(res){
                var data = res.body;
                // 错误处理
                if (data.status != 0) {
                    Toast(data.message);
                    return ;
                }
                this.list = data.message;
            })
        }
    }
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
	.mui-icon-home:before,
	.mui-icon-email:before,
	.mui-icon-chatbubble:before,
	.mui-icon-location:before,
	.mui-icon-search:before,
	.mui-icon-phone:before{
		content: '';
		display: inline-block;
		width: 50px;
		height: 50px;
		background-repeat: round;
	}

	.mui-icon-home:before{
		background-image: url(../../statics/imgs/1.png);
	}

	.mui-icon-email:before{
		background-image: url(../../statics/imgs/2.png);
	}
	.mui-icon-chatbubble:before{
		background-image: url(../../statics/imgs/3.png);
	}
	.mui-icon-location:before{
		background-image: url(../../statics/imgs/4.png);
	}
	.mui-icon-search:before{
		background-image: url(../../statics/imgs/5.png);
	}
	.mui-icon-phone:before{
		background-image: url(../../statics/imgs/6.png);
	}
</style>
