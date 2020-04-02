<template>
	<view class="todo-list" @touchstart="handleStart" 
							@touchend="handlEnd">
		<view :class="{'todo-content': true, 'done-content': item.done}" v-for="item in ListData" :key="item.id" 
		@click="switch_status(item.id)">
			<view class="todo-checkbox"></view>
			<view class="todo-text">{{item.content}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			  list: this.$root.list
			};
		},
		computed: {
			ListData(){
				let list = JSON.parse(JSON.stringify(this.$root.list))
				let newlist = []
				if(this.$root.ActieIndex === 0) {
					this.$emit('input', list.length)
					return list
				}
				if(this.$root.ActieIndex === 1) {
					list.forEach((item) => {
						if(!item.done) {
							newlist.push(item)
						}
					})
					this.$emit('input', newlist.length)
					return newlist
				}
				if(this.$root.ActieIndex === 2) {
					list.forEach((item) => {
						if(item.done) {
							newlist.push(item)
						}
					})
					this.$emit('input', newlist.length)
					return newlist
				}
				return []
			}
		},
		methods: {
			// switch todo/done
			switch_status(id) {
				var index = this.$root.list.findIndex((item)=>item.id === id)
				this.list[index].done = !this.list[index].done
				var _this = this.$root
				uni.request({
					url: getApp().globalData.url + "switch",
					method: "POST",
					data: {
						usr: getApp().globalData.usr,
						pwd: getApp().globalData.pwd,
						eventlis: {
							id: _this.list[index].id,
							done: _this.list[index].done
						}
					}
				})
			},
			handleStart(event) {
				this.$root.handleStart(event)
			},
			handlEnd(event) {
				this.$root.handlEnd(event)
			}
		}
	}
</script>

<style lang="stylus" scoped>
	// 事项显示样式
	.todo-list 
		padding-top 50px
		.todo-content
			position relative
			padding 15px
			margin 15px
			display flex
			align-items center
			background #64c6fb
			font-size 14px
			border-radius 10px
			box-shadow: 2px 2px 10px 2px #cccccc, 2px 2px 10px 1px #477eff inset
			overflow hidden
			.todo-checkbox
				width 20px
				height 20px
				background #FFFFFF
				margin-right 15px
				border-radius 50%
				box-shadow 0 0 2px 2px #cccccc
				flex-shrink 0
			.todo-text
				word-break: break-all;
				white-space: normal;
				width:auto
				font-family Arial,Helvetica,sans-serif
				font-size 14px
				color:#5381ff;
				font-weight 600
				text-transform uppercase
				
				
		.todo-content:after
			position: absolute
			top 0
			bottom 0
			left 0
			width 10px
			content ''
			border-radius 50px
			background #0062ff
		.done-content.todo-content:after
			background #00aaff
		.done-content .todo-checkbox
			background #eee	
		.done-content .todo-text
			text-decoration line-through
			color #7e7e7e	
</style>
