<template>
    <transition name="move" >
        <div v-show="showFlag" class="food" ref="food">
            <div class="food-content">
                <div class="image-header">
                    <img :src="food.image">
                    <div class="back" @click="hide">
                        <i class="icon-arrow_lift"></i>
                    </div>
                </div>
                <div class="content">
                    <h1 class="title">
                        {{food.name}}
                    </h1>
                    <div class="details">
                        <span class="sell-count">月售{{food.sellCount}}份</span>
                        <span class="rating">好评率{{food.rating}}</span>
                    </div>
                    <div class="price">
                        <span class="now">
                            ￥{{food.price}}
                        </span>
                        <span class="old" v-show="food.oldPrice">
                            ￥{{food.oldPrice}}
                        </span>
                    </div>
                    <div class="cartcontrol-wrapper">
                      <cartcontrol :food="food" @cartAdd="add"></cartcontrol>
                    </div>
                    <transition name="fade">
                        <div class="buy " v-show="!food.count || food.count===0" @click.stop.prevent="addFirst">
                            加入购物车
                        </div>
                     </transition>
                </div>
                <split v-if="food.info"></split>
                <div class="info" v-if="food.info">
                    <h1 class="title">商品介绍</h1>
                    <p class="text">{{food.info}}</p>
                </div>
                <split></split>
                <div class="rating">
                    <h1 class="title">商品评价</h1>
                    <ratingSelect :selectType="selectType" :onlyContent="onlyContent" :desc="desc" :rating="food.ratings"></ratingSelect>
                </div>
            </div>
        </div>
    </transition>
</template>
<script type="ecmascript-6">
    import store from "../store.js";
    import {mapState} from 'vuex';
    import BScroll from "better-scroll";
    import Vue from "vue";
    import cartcontrol from './cartcontol.vue';
    import ratingSelect from './ratingSelect.vue';
    import split from './split.vue';
    export default{
        props:['food'],
        data(){
            return{
                showFlag:false,
                selectType:2,
                onlyContent:true,
                desc:['全部','推荐','吐槽']
            }
        },
        methods:{
            show(){
                this.showFlag=true;
                this.selectType=0;
                this.$nextTick(() => {
                  if (!this.scroll) {
                    this.scroll = new BScroll(this.$refs.food, {
                      click: true
                    });
                  } else {
                    this.scroll.refresh();
                  }
                });
            },
            hide(){
                this.showFlag=false;
            },
            addFirst(event){
                if(!event._constructed){
                    return;
                }
                this.$emit('cartAdd',event.target);
                Vue.set(this.food,'count',1);
            },
            add(target) {
               this.$emit('cartAdd',target);
            }
        },
        components:{
            cartcontrol,
            split,
            ratingSelect
        }
    }
</script>

<style rel="stylesheet" lang="scss">
    .food{
        position:fixed;
        left:0;
        top:0;
        bottom:48px;
        z-index:30;
        width:100%;
        background:#fff;
        transform:translate3d(0,0,0);
        transition:all 0.2s linear;
        &.move-enter,&.move-leave{
            transform:translate3d(100%,0,0);
        }
        .image-header{
            position:relative;
            width:100%;
            height:0;
            padding-top:100%;
            img{
                position:absolute;
                top:0;
                left:0;
                width:100%;
                height:100%;
            }
            .back{
                position:absolute;
                top:10px;
                left:0;
                .icon-arrow_lift{
                    display:block;
                    padding:10px;
                    font-size:20px;
                    color:#fff;
                }
            }
        }
        .content{
            padding:18px;
            position:relative;
            .title{
                line-height:14px;
                margin-bottom:8px;
                font-size:14px;
                font-weight:700;
                color:rgb(7,17,27);
            }
            .details{
                margin-bottom:18px;
                line-height:10px;
                font-size:0;
                height:10px;
                .sell-count,.rating{
                    font-size:10px;
                    color:rgb(147,153,159);
                }
                .sell-count{
                    margin-right:12px;
                }
            }
            .price{
                    font-weight:700;
                    line-height:24px;
                    .now{
                        margin-right:8px;
                        font-size:14px;
                        color:rgb(240,20,20);
                    }
                    .old{
                        text-decoration:line-through;
                        font-size:10px;
                        color:rgb(147,153,159);
                    }
                }
            .cartcontrol-wrapper{
                position:absolute;
                right:12px;
                bottom:12px;
            }
            .buy{
                position:absolute;
                right:18px;
                bottom:18px;
                z-index:10;
                height:24px;
                line-height:24px;
                padding:0 12px;
                box-sizing:border-box;
                font-size:10px;
                border-radius:12px;
                color:#fff;
                background:rgb(0,160,220);
                opacity:1;
                transition:all 0.2s;
                &.fade-enter,&.fade-leave{
                    opacity:0;
                }
            }
        }
        .info{
            padding:18px;
            .title{
                line-height:14px;
                margin-bottom:6px;
                font-size:14px;
                color:rgb(7,17,27);
            }
            .text{
                line-height:24px;
                padding:0 8px;
                font-size:12px;
                color:rgb(77,85,93);
            }
            
        }
        .rating{
            padding-top:18px;
            .title{
                line-height:14px;
                margin-left:18px;
                font-size:14px;
                color:rgb(7,17,27);
            }
        }

    }
</style>