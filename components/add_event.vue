<template>
	<view>
		<!-- 添加事项模块 -->
		<view class="add-todo" @click="click_to_show">
			<text class="iconfont icon-add" :class="{'add_open':show}"></text>
		</view>
		<view class="add-todo-content" :class="{'content_open':show}">
			<input class="add-input" focus="true" placeholder="请输入你的todo事项" v-model="content" v-if="show"/>
			<view class="add-button" @click="createvent">
				创建
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				show: false,
				content: ''
			};
		},
		methods: {
			// create event
			createvent() {
				if(!this.content) {
					uni.showToast({
						title: "输入内容不能为空哦",
						icon: "none"
					})
				}
				else {
					this.$root.list.unshift({
						content: this.content,
						id: new Date().getTime(),
						done: false
					})
					this.content = ''
				}
			},
			// click to show
			click_to_show() {
				this.show = !this.show
			}
		}
	}
</script>

<style lang="stylus" scoped>
	@import url('../common/iconfont.css');
// 添加事项样式
	.add-todo
		position fixed
		display flex
		justify-content center
		align-items center
		width 50px
		height 50px
		box-shadow 0 0 10px 4px rgba(0, 0, 0, 0.1)
		border-radius 50%
		background #deeff5
		bottom 25px
		left 0
		right 0
		margin 0 auto
		.icon-add
			font-size 30px
			position absolute
			color #add8e6
			transition transform 0.3s
		.add_open
			transform rotate(135deg)
	.add-todo-content
		position fixed
		display flex
		align-items center
		left 20px
		right 20px
		bottom 100px
		padding 10px 15px
		border-radius 50px
		background #add8e6
		box-shadow 0 0 5px 3px rgba(0, 0, 0, 0.1)
		transition all 0.3s
		opacity 0
		transform scale(0) translateY(200%)
	.content_open
		opacity 1
		transform scale(1) translateY(0)
		.add-input
			width 100%
			color #808080
		.add-button 
			flex-shrink 0
			background #678acb
			border-radius 50px
			font-size 16px
			padding 8px
			color #CCCCCC
			box-shadow 0 0 5px 1px #FFFFFF
</style>
</style>
