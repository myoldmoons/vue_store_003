<template>
	<div class="temp">
		<!--1.0 商品轮播图-->
		<div class="slider">
			<slider :imgs="imgs"></slider>
		</div>

		<!--2.0 实现商品购买区-->
		<div id="buy">
			<h4 v-text="info.title"></h4>
			<p class="line"></p>
			<ul>
				<li class="price">
					市场价: <s>￥{{info.market_price}}</s>    销售价：<span>￥{{info.sell_price}}</span>
				</li>
				<li class="inputli">
					购买数量： <inputnumber v-on:dataobj="getcount" class="inputnumber"></inputnumber>
					<transition name="show" @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">
						<div v-if="isshow" class="ball"></div>
					</transition>
				</li>
				<li>
					<mt-button type="primary" size="small">立即购买</mt-button>
					<mt-button type="danger" size="small" @click="toshopcar">加入购物车</mt-button>
				</li>
			</ul>
		</div>

		<div id="params">
			<h6>商品参数</h6>
			<p class="line"></p>
			<ul>
				<li>商品货号：{{info.goods_no}}</li>
				<li>库存情况：{{info.stock_quantity}}</li>
				<li>上架时间：{{info.add_time | datefmt('YYYY-MM-DD')}}</li>
			</ul>
		</div>

		<!--3.0 图文详情-->
		<!--4.0 商品评论-->
		<div id="other">
			<router-link v-bind="{to:'/goods/goodsdesc/'+id}">
				<mt-button class="imgdesc" type="primary" size="large">图文详情</mt-button>
			</router-link>
			<router-link v-bind="{to:'/goods/goodscomment/'+id}">
				<mt-button type="danger" size="large">商品评论</mt-button>
			</router-link>
		</div>
	</div>
</template>


<script>
import { Toast } from 'mint-ui';
import common from '../../kits/common.js';
import {vm, COUNTSTR} from '../../kits/vm.js';
import {setItem, valueObj} from '../../kits/localSG.js';
import slider from '../subcom/slider.vue';
import inputnumber from "../subcom/inputNumber.vue";

export default {
	components : {
		slider,
		inputnumber
	},
	data(){
		return {
			id : 0,
			imgs : [],
			info : {},
			isshow : false,  // 控制小球的隐藏状态
			inputNumberCount : 1
		}
	},
	created(){
		this.id = this.$route.params.id;
		this.getImgs();
		this.getinfo();
	},
	methods : {
		beforeEnter(el){
			el.style.transform = "translate(0px,0px)";
		},
		enter(el,done){
			el.offsetWidth;
			el.style.transform = "translate(75px,366px)";
			done();
		},
		afterEnter(el){
			this.isshow = !this.isshow;
		},
		getImgs(){
			var url  = common.apidomain + '/api/getthumimages/'+this.id;
			this.$http.get(url).then(function(res){
				var data = res.body;
				// 错误处理
				if(data.status != 0){
					Toast(data.message);
					return;
				}
				//判断状态
				this.imgs = data.message;
			})
		},
		getinfo(){
			var url = common.apidomain +'/api/goods/getinfo/'+this.id;
			this.$http.get(url).then(function(res){
				//判断状态
				this.info = res.body.message[0];
			});
		},

		getcount(count){
			this.inputNumberCount = count;
		},

		toshopcar(){
			vm.$emit(COUNTSTR, this.inputNumberCount);


			// export var valueObj = {goodsid:88, count：5};
			valueObj.goodsid = this.id;  // 商品的id
			valueObj.count = this.inputNumberCount;  // 商品的购买数量
			// console.log(valueObj.count);
			setItem(valueObj);

			this.isshow = !this.isshow;
		}
	}
}
</script>


<style scoped>
	.silder{
		border:1px solid rgba(0,0,0,0.4);
		border-radius: 5px;
		margin: 5px;
	}

	#buy, #params, #other{
		margin: 5px;
		padding: 5px;
		border:1px solid rgba(0,0,0,0.4);
		border-radius: 5px;
	}
	.line{
		height: 1px;
		border: 1px solid rgba(0,0,0,0.2);
	}
	#buy ul, #params ul{
		padding-left: 0px;
	}
	#buy h4{
		color:#0094ff;
		padding: 5px;
	}
	#buy li, #params li{
		list-style: none;
		padding: 8px;
	}

	#buy .price span{
		color:red;
	}

	#other .imgdesc{
		margin-bottom: 20px;
	}
	.inputli{
		position: relative;
	}
	.inputnumber{
		position: absolute;
		left:100px;
		top:5px;
	}

	.ball{
		background-color: red;
		height: 20px;
		width: 20px;
		border-radius: 50%;
		position: absolute;
		left:150px;
		top:10px;
		transition: all 0.4s ease;
		z-index: 100;
	}
</style>
