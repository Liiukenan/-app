<template>
    <div class="rating-select">
    	<div class="rating-type">
    		<span class="block positive" :class="{'active':selectType===0}" @click="select(0,$event)">{{desc[0]}} <span class="count">47</span></span>
    		<span class="block positive" :class="{'active':selectType===1}" @click="select(1,$event)">{{desc[1]}} <span class="count">40</span></span>
    		<span class="block negative" :class="{'active':selectType===2}" @click="select(2,$event)">{{desc[2]}} <span class="count">7</span></span>
    	</div>
    	<div class="switch" :class="{'on':onlyContent}" @click="toggleContent">
    		<span class="icon-check_circle">
    		</span>
    		<span class="text">
    			只看有内容的评价
    		</span>
    	</div>
    </div>
</template>

<script type="ecmascript-6">
// const POSITIVE=0;
// const NEGATIVE=1;
// const ALL=2;
    export default{
    	props:['ratings','selectType','onlyContent','desc'],
    	data(){
    		return{
    			
    		}
    	},
    	methods:{
    		select(type,event){
    			if(!event._constructed){
    				return;
    			}
    			this.selectType=type;

    			this.$emit('ratingtype',type);
    		},
    		toggleContent(event){
    			// if(!event._constructed){
    			// 	return;
    			// }
    			// this.onlyContent=!this.onlyContent;
    			// this.$emit('countToggle',this.onlyContent);
    		}
    	}
    }
</script>

<style rel="stylesheet" lang="scss">
@import '../css/base.scss';
    .rating-select{
    	.rating-type{
    		padding:18px 0;
    		margin:0 18px;
    		padding:18px 0;
    		@include border-1px(rgba(7,17,27,0.1));
    		font-size:0;
    		.block{
    			display:inline-block;
    			padding:8px 12px;
    			margin-right:8px;
    			border-radius:1px;
    			line-height:16px;
    			color:rgb(77,85,93);
    			font-size:12px;
    			.count{
    				margin-left:2px;
    				font-size:8px;
    			}
    			&.active{
    				color:#fff;
    			}
    			&.positive{
					background:rgba(0,160,220,0.2);
					&.active{
						background:rgb(0,160,220);
					}
    			}
    			&.negative{
    				background:rgba(77,85,93,0.2);
    				&.active{
    					background:rgb(77,85,93);
    				}
    			}
    		}
    	}
    	.switch{
    		padding:12px 18px;
    		line-height:24px;
    		border-bottom:1px solid rgba(7,17,27,0.1);
    		color:rgb(147,153,159);
    		font-size:0;
    		&.on{
    			.icon-check_circle{
					color: #00c850;
    			}
    		}
    		.icon-check_circle{
    			display:inline-block;
    			vertical-align:top;
				margin-right:4px;
				font-size:24px;
    		}
    		.text{
    			display:inline-block;
    			vertical-align:top;
    			font-size:12px;
    		}
    	}
    }
</style>