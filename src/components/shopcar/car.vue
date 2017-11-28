<template>
	<div class="temp">
		<div class="row" v-for="(item,index) in datalist" :keys="item.id">
			<mt-switch class="switch" v-model="value[index]"></mt-switch>
			<img class="img" :src="item.thumb_path" alt="商品图片" >
			<div class="inforight">
				<h4 v-text="item.title"></h4>
				<div class="bottom">
					<ul>
						<li>￥{{item.sell_price}}</li>
						<li class="one"><carinputnumber :initCount="item.cou" :goodsid="item.id" @cardataobj="getiInputNumber"></carinputnumber></li>
						<li><a href="javascript:void(0)" @click="delrow(item.id,index)">删除</a></li>
					</ul>
				</div>
			</div>
		</div>

		<div class="total">
			<div class="desc">
				<ul>
					<li>总计（不含运费）</li>
					<li>已勾选商品{{totalcount}}件,总价:￥{{totalAmount}}元</li>
				</ul>
			</div>
			<div class="button">
				<mt-button  type="danger" size="small">去结算</mt-button>
			</div>
		</div>
		{{value}}
	</div>
</template>

<script>
import common from '../../kits/common.js';
import {vm, COUNTSTR} from '../../kits/vm.js';
import { Toast } from 'mint-ui';
import { getgoodsObject, updateData, remoteItem } from "../../kits/localSG.js";
import carinputnumber from '../subcom/carinputnumber.vue'
export default {
	components : {
		carinputnumber
	},
	computed : {
        totalcount(){
            var trueArr = this.value.filter(function(item){ return item });

            var amount = 0;

            this.value.forEach((item, index) => {
                if (item) {
                    var count = this.datalist[index].cou;
                    var price = this.datalist[index].sell_price;
                    var itemamount = count * price;
                    amount += itemamount;
                }
            })

            this.totalAmount = amount;
            return trueArr.length;
        }
    },
    data(){
		return {
			totalAmount:0, //总价格
			value:[],
			num : 0,
			datalist:[
				// {
				// 	"cou": 1,
				// 	"id": 87,
				// 	"title": "华为（HUAWEI）荣耀6Plus 16G双4G版",
				// 	"sell_price": 2195,
				// 	"thumb_path": "http://182.254.146.100:8080/upload/201504/16/201504161218551181.jpg"
				// }
			],
			two : 0
		}
	},
	created(){
		this.getdatalist();
	},
	methods : {
		delrow(goodsid, index){
            this.value.splice(index, 1);
            this.datalist.splice(index, 1);
            remoteItem(goodsid);
			// this.getdatalist();
        },

		getdatalist(){
			// 从localStorage中获取到素有的商品id值 {88： 5}
			var obj = getgoodsObject();
			// 将id值按照api的参数格式提交给API
			var idstring = "";
			// idstring = idstring || 0;

			for (var key in obj){
				idstring += key + ','
			}

			// 将最后一个 "," 去掉
			idstring = idstring.substring(0,idstring.length - 1) || 0;

			var url  = common.apidomain + '/api/goods/getshopcarlist/'+idstring;

			this.$http.get(url).then(function(res){
					//状态值的判断
					if(res.body.status != 0 ){
						Toast(res.body.message);
						return;
					}
					//	将locaStorage中的所有的商品对应的count的值赋值给message中每个对象的cou
					res.body.message.forEach((item)=>{

						item.cou = obj[item.id];




					//	为了解决一个bug，所以必须在此处初始化value数组全部为false
						this.value.push(false);
					});

					// console.log("111 === " + this.two);

					this.datalist = res.body.message;

				});
		},
		getiInputNumber(resObj){
			// resObj = {"goodsid" : 89, "type" : "add"}
			// console.log(resObj);
			// 更新localStorage中的数据
			updateData(resObj);

			// 更新this.datalist中的当前数量
			for(var i = 0;i <this.datalist.length ; i++){
				if(this.datalist[i].id == resObj.goodsid){
					if(resObj.type =='add'){
						this.datalist[i].cou = this.datalist[i].cou + 1;
					}else{
						this.datalist[i].cou = this.datalist[i].cou - 1;
					}
					break;
				}
			}


		}
	}
}
</script>

<style lang="css">
    .row{
        border-bottom: 1px solid rgba(0,0,0,0.3);
        height: 102px;
        display: flex;
    }

    .switch{
        flex:0 0 52px;
    }

    .img{
        margin-left: 5px;
        height: 75px;
        width: 75px;
        flex: 0 0 85px;
    }

    .inforight{
        margin-left: 5px;
        flex:1;
    }
    .inforight ul{
        padding-left: 0px;
    }
    .inforight li{
        list-style: none;
        display: inline-block;
    }

    .inforight h4{
        color: #0094ff;
        font-size: 14px;
    }

    .bottom li:first-child{
        color:red;
        margin-right: 10px;
    }

    .bottom li:last-child{
        margin-left: 10px;
    }
    .total{
        height: 100px;
        background-color: rgba(0,0,0,0.1);
        display: flex;
        padding: 5px;;
    }

    .total ul {
        padding-left: 0px;
    }
    .total li{
        list-style: none;
        font-size: 14px;
    }

    .button{
        flex:0 0 60px;
        margin: 30px 0 0 0 ;
    }

    .desc{

        flex:1;
    }
</style>
