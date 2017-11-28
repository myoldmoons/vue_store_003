<!-- 以后项目的根组件 -->
<template>
	<div>
		<!-- 1.0 利用mint-ui中的header组件四线整个系统的头部 -->
		<mt-header fixed title="Vue项目商场"></mt-header>
		<!-- 2.0 利用vue-route的<router-view>进行占位 -->
		<router-view></router-view>

		<div v-if="isshow" class="back">
            <a href="javascript:void(0)" @click="backto">返回</a>
        </div>


		<!-- 3.0 利用mui中的tabbar组件实现系统的地步 -->
		<nav class="mui-bar mui-bar-tab">
            <router-link class="mui-tab-item" to="/home">
                <span class="mui-icon mui-icon-home"></span>
                <span class="mui-tab-label">首页</span>
            </router-link>
            <router-link class="mui-tab-item" to="/tabbar-with-chat">
                <span class="mui-icon mui-icon-email"></span>
                <span class="mui-tab-label">会员</span>
            </router-link>
            <router-link class="mui-tab-item" to="/shopcar/car">
                <span class="mui-icon mui-icon-contact">
                    <span  id="badge" class="mui-badge">0</span>
                </span>
                <span class="mui-tab-label">购物车</span>
            </router-link>
            <router-link class="mui-tab-item" to="/tabbar-with-map">
                <span class="mui-icon mui-icon-gear"></span>
                <span class="mui-tab-label">搜索</span>
            </router-link>
        </nav>
	</div>
</template>

<script>
	import {vm, COUNTSTR} from './kits/vm.js';
	import { getgoodsObject} from "./kits/localSG.js";

	vm.$on(COUNTSTR, function(count){
		var badgeobj = document.querySelector('#badge');
		badgeobj.innerText = parseInt(badgeobj.innerText) + count;
	})


	export default{
		data(){
	        return {
	            isshow : false
	        }
	    },
	    beforeCreate(){
			setTimeout(function(){
	    		var haha = getgoodsObject();
				var num = 0;
				for (var key in haha){

					num += haha[key];
				}
				console.log(num);
				vm.$emit(COUNTSTR, num);
	    	},500)
		},
		methods : {
	        backto : function(){
	            this.$router.go(-1)
	        }
	    },
		watch : {
	        "$route" : function(newroute, oldroute){
	            if (newroute.path.toLowerCase() == "/home") {
	                this.isshow = false;
	            }else {
	                this.isshow = true;
	            }
	        }
	    }
	}
</script>

<style scoped>
	.red{
		color : red
	}
	.back{
		width: 60px;
		position: absolute;
		top:10px;
		left:10px;
		z-index: 101;
	}
	.back a{
		color:white;
		font-size: 16px;
		font-weight: bold;
	}
</style>
