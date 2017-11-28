<template lang="html">
    <div class="sub">
        <div class="inleft div" @click="substrict">-</div>
        <div class="incenter div" v-text="initCount"></div>
        <div class="inright div" @click="add">+</div><br>
    </div>
</template>

<script>

import {vm, COUNTSTR} from '../../kits/vm.js';

export default {
    props: ['initCount', 'goodsid'],
    data(){
        return {
            count : 1,
            resObj: {goodsid: 0, type: 'add'}
        }
    },
    created(){
        this.count = this.initCount;
    },
    methods : {
        substrict(){
            this.count --;
            if (this.count < 1) {
                this.count = 1;
                return;
            }
            vm.$emit(COUNTSTR, -1);
            this.sendmessage("substrict");
        },
        add(){
            this.count ++;
            vm.$emit(COUNTSTR, 1);
            this.sendmessage('add');
        },
        sendmessage(type){
            this.resObj.type = type;
            this.resObj.goodsid = this.goodsid;

            // {"goodsid" : 89, "type" : "add"}
            this.$emit('cardataobj',this.resObj);
        }
    }
}
</script>

<style lang="css" scoped>
    .sub .div{
        width: 40px;
        height: 25px;
        line-height: 25px;
        border: 1px solid #000;
        display: inline-block;
        text-align: center;
    }
</style>
