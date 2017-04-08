<template>
	<div>
		<div class="leftSlip-wrap"  v-for="(item,index) in list" >
			<div class="left-slip-container" :style="item.txtStyle" 
			@touchstart="onSlipTouchStart($event)"
			@touchmove="onSlipTouchMove($event)"
			@touchend="onSlipTouchEnd($event)"
			:data-index="index">
			{{item.text}}
			</div>
			<div class="del" @click="onDeleteItem(index)">删除</div>
		</div>
	</div>
</template>
<script>
	export default{
		props:[
			'list'
		],
		data (){
			return {
				startX:0,	  //  按下的位置
				moveX:0,	 //   鼠标移动时水平方向的位置
				disX:0,		//	  移动之间的距离
				delWidth:5.25
			}
		},
		mounted () {
		},
		methods: {
			onSlipTouchStart (e) {
				if(e.touches.length == 1){
					this.startX = e.touches[0].clientX;    
				}
			},
			onSlipTouchMove (e) {
				if(e.touches.length == 1){
					this.moveX = e.touches[0].clientX
					this.disX = this.startX - this.moveX;

					let delWidth = this.delWidth;
					let txtStyle = '';

					if (this.disX > 0 || this.disX == 0) {
				        txtStyle = "left:0"
				    } else if (this.disX > 0) {
				        txtStyle = "left:-" + disX + "rem";
				        if (this.disX >= delWidth) {
				          txtStyle = "left:-" + delWidth + "rem";
				        }
				    }
				    let idx = e.currentTarget.dataset.index;
				    let list = this.list;
				    list[idx].txtStyle = txtStyle;
				}
			},
			onSlipTouchEnd (e) {
				if (event.changedTouches.length == 1) {
			      	// 手指移动结束后的水平位置
			      	let endX = event.changedTouches[0].clientX;
			      	// 触摸开始与结束,手指移动的距离
			      	let disX = this.startX - endX;
			      	let delWidth = this.delWidth;
			      	//如果距离小于删除按钮的1/2，不显示删除按钮
			      	let txtStyle = disX > delWidth / 2 ? "left:-" + delWidth + "rem" : "left:0";
			      	//获取手指触摸的是哪一项
			      	let idx = event.currentTarget.dataset.index;
			      	let list = this.list;
			      	list[idx].txtStyle = txtStyle;
			      	// this.$emit('on-change',list)
			    }
			},
			onDeleteItem(index){
				this.list.splice(index,1)
				this.$emit('on-change',this.list)
			}

		}
	}
</script>
<style scoped>
	.leftSlip-wrap{
		display: flex;
		flex-direction: row;
		position: relative;
		height: 3.5rem;
		border-bottom: 1px solid #eaeaea;
	}
	.left-slip-container{
		width: 100%;
		height: 3rem;
		background: #fff;
		position: absolute;
		left: 0;
		z-index: 1;
		display: flex;
		flex-direction: row;
		padding-top: 0.5rem;
	}
	.del{
		background:#ff7676;
		color:#fff;
		width:5.25rem;
		height: 3.5rem;
		line-height:3.5rem;
		text-align:center;
		font-size:0.85rem;
		position:absolute;
		right:0;
		top: 0;
		z-index:0;
	}
	.project-name{
	    color: #656565;
	    margin-top: 0.25rem;
	}
	.project-price{
	    color: #fc6d6f;
	    margin-top: 5px;
	}
	.project-info{
	    margin-left: 0.5rem;
	    display: flex;
	    flex-direction: column;
	}
	.item-img{
	    width: 2.5rem;
	    height: 2.5rem;
	    border-radius: 50%;
	    margin-left: 0.75rem;
	}
</style>