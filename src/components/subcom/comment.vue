<template lang="html">
    <div class="temp">
        <!-- 评论组件 -->
        <div class="postcomment">
            <h3>提交评论</h3>
            <p class="p"></p>
            <textarea placeholder="请输入您要评论的内容..." v-model="postcontent"></textarea>
            <mt-button type="primary" size="large" @click="postcomment">发表</mt-button>
        </div>

        <!-- 2.0实现获取评论数据列表 -->
        <div class="list">
            <h3>评论列表</h3>
            <p class="p"></p>
            <div v-for="(item, index) in list" class="title">
                <div class="title">
                    <span>第{{index + 1}}楼</span>
                    <span>用户:{{item.user_name}}</span>
                    <span>发表时间:{{item.add_time | datefmt('YYYY-MM-DD HH:mm:ss')}}</span>
                </div>
                <ul class="mui-table-view">
                    <li v-text="item.content" class="mui-table-view-cell"></li>
                </ul>
            </div>
        </div>

        <!--3.0 实现获取更多按钮-->
		<mt-button type="danger" size="large" plain @click="getmore">加载更多</mt-button>
    </div>
</template>

<script>
    import { Toast } from 'mint-ui';
    import common from '../../kits/common.js';
    export default {
        data(){
            return{
                // 用户自动获取用户评论内容
                postcontent : '',
                // 评论数组的数据
                list : [],
                // 代表获取api中的第几页评论数据
                pageindex : 1
            }
        },
        created(){
            this.getcommentlist(this.pageindex);
        },
        props : ['id'],
        methods : {
            // 第一个功能：判断用户是否填写了数据
            postcomment(){
                // 判断用户是否天界评论内容
                if (this.postcontent.trim().length <= 0) {
                    Toast("您要评论的内容为空");
                    return;
                }
                // 确定提交的地址 /api/postcomment/:artid
                var url = common.apidomain + '/api/postcomment/' + this.id;
                // 利用ajax的post请求将数据提交到指定的地址
                // 获取到用户在文本框中填写的评论内容，通过 content: 内容 格式提交到请求报文体中
                // 查看是否评论成功 http://182.254.146.100:8899/api/getcomments/13?pageindex=1
                this.$http.post(url, {content : this.postcontent}, {emulateJSON:true})
                    .then(function(res){
                        Toast(res.body.message);

                        this.list = [
                            {
                                'user_name' : '匿名用户',
                                'add_time' : new Date(),
                                'content' : this.postcontent
                            }
                        ].concat(this.list);

                        console.log(this.list);

                        this.postcontent = '';
                        // 将最新的评论数据追加到评论列表的最顶部
                        // this.getcommentlist(this.pageindex)
                    })
            },

            // 第二个功能：获取评论的数据
            // 获取当前数据的评论数据 pageindex:代表的是当前获取的是哪一页的数据，默认值是1
            getcommentlist(pageindex){
                pageindex = pageindex || 1;
                // 确定评论数据的url
                var url = common.apidomain + '/api/getcomments/' + this.id + '?pageindex=' + pageindex;
                // 发出ajax请求获取数据
                this.$http.get(url).then(function(res){
                    var body = res.body;
                    if (body.status != 0) {
                        Toast(body.message);
                        return;
                    }
                    // 将message数组中的数据赋值给this.list
                    console.log(body.message);
                    this.list = this.list.concat(body.message)
                })
            },


            // 第三个功能: 实现加载更多
            getmore(){
                this.pageindex ++;
                this.getcommentlist(this.pageindex);
            }
        }
    }
</script>

<style lang="css" scoped>
    /* 1.0 实现提交评论样式 */
    .postcomment{
        padding: 5px;
    }
    .p{
        height: 1px;
        width: 100%;
        border-bottom: 1px solid rgba(0,0,0,0.3);
    }

    /*2.0 评论列表的样式*/
    .list{
        padding: 5px;
    }
    .title{
        padding: 5px;
        color: #6d6d72;
        font-size: 15px;
        background-color: rgba(0,0,0,0.1);
    }
</style>
