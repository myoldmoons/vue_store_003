<template>
    <div class="temp">
        <ul class="mui-table-view">
            <li v-for="item in list" class="mui-table-view-cell mui-media">
                <router-link v-bind="{to:'/news/newsinfo/' + item.id}">
                    <img class="mui-media-object mui-pull-left" :src="item.img_url">
                    <div class="mui-media-body">
                        {{item.title}}
                        <p class='mui-ellipsis' v-text='item.zhaiyao'></p>

                        <div class="ft">
                            <span>发表时间:{{item.add_time | datefmt('YYYY-MM-DD HH:mm:ss')}}</span>
                            <span>点击数:{{item.click}}</span>
                        </div>
                    </div>
                </router-link>
            </li>
        </ul>
    </div>
</template>

<script>
import { Toast } from 'mint-ui';
import common from '../../kits/common.js';
export default {
    data(){
        return {
            list : [
                // {
                //     "id": 13,
                //     "title": "1季度多家房企利润跌幅超50% 去库存促销战打响",
                //     "add_time": "2015-04-16T03:50:28.000Z",
                //     "zhaiyao": "房企一季度销售业绩已经陆续公布，克而瑞研究中心统计",
                //     "click": 1,
                //     "img_url": "http://182.254.146.100:8080/upload/201504/16/201504161149414479.jpg"
                // },
                // {
                //     "id": 14,
                //     "title": "买房还是炒股，2015年买房无法拒绝的5大理由",
                //     "add_time": "2015-04-16T04:05:34.000Z",
                //     "zhaiyao": "转眼间2015年已经过去了4个月，在这短短的四个月",
                //     "click": 2,
                //     "img_url": "http://182.254.146.100:8080/upload/201504/16/201504161205596364.jpg"
                // }
            ]
        }
    },
    created(){
        this.getnewlist();
    },
    methods:{
        getnewlist(){
            var url = common.apidomain + '/api/getnewslist';

            this.$http.get(url).then(function(res){
                var data = res.body;
                if (data.status != 0) {
                    Toast(data.message);
                    return;
                }
                this.list = data.message
            })
        }
    }
}
</script>

<style scoped>
    .mui-table-view img{
        height:80px;
        width:80px;
    }

    .mui-table-view .mui-media-object{
        max-width: 80px;
        line-height: 80px;
    }

    .ft{
        margin-top: 1.5em;
        font-size: 14px;
        color:#0094ff;
    }

    .ft .click{
        margin-left: 20px;
    }
</style>
