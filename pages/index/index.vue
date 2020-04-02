<template>
	<view class="content">
		<!-- 状态栏组件 -->
		<view class="background" @click="handleClick" @touchstart="handleStart" @touchend="handleEnd"></view>
		<view class="header">
			<view class="header-left">
				<text class="active-content">{{tab_text}}</text>
				<text class="active-content">{{tab_num}}</text>
			</view>
			<view class="header-right">
				<text class="header-right-content" :class="{'active-content': ActieIndex === 0}" @click="SwitchTab(0)">全部</text>
				<text class="header-right-content" :class="{'active-content': ActieIndex === 1}" @click="SwitchTab(1)">代办</text>
				<text class="header-right-content" :class="{'active-content': ActieIndex === 2}" @click="SwitchTab(2)">已完成</text>
			</view>
		</view>
		<!-- 事项显示组件 -->
		<show_event v-model="tab_num"></show_event>
		<!-- 添加事项组件 -->
		<add_event ref="addEvent"></add_event>
	</view>
</template>

<script>
	import show_event from '@/components/show_event.vue'
	import add_event from '@/components/add_event.vue'
	export default {
		components: {
			show_event,
			add_event
		},
		data() {
			return {
				tab_num: 0,
				tab_text: '代办',
				ActieIndex: 1,
				list: [],
				lastX: 0,
				lastY: 0,
				scroll: false,
				direction: ''
			}
		},
		onLoad() {
			uni.request({
				url: getApp().globalData.url + "course",
				method: "POST",
				data: {
					usr: getApp().globalData.usr,
					pwd: getApp().globalData.pwd
				},
				success: (res) => {
					if(res['data']['data']) {
						var Day_lis = ["Sun", "Mon", "Tus", "Wed", "Thu", "Fri", "Sat"]
						var course_lis = res.data.data[Day_lis[new Date().getDay()]]
						course_lis.forEach((item,index) => {
							if(item) {
								this.list.push({
									content: item,
									id: index.toString() + new Date().getDay(),
									done: false
								})
							}
						})
						uni.showToast({
							title: "课表获取成功,今天也是爱你的一天~",
							icon: "none"
						})
					}
				},
				fail(res) {
					uni.showToast({
						title: "哟呵,课表获取失败~",
						icon: "none"
					})
				}
			})
			uni.request({
				url: getApp().globalData.url + "event",
				method: "POST",
				data: {
					usr: getApp().globalData.usr,
					pwd: getApp().globalData.pwd,
					method: "get"
				},
				success: (res) => {
					var eventlis = res.data.eventlis
					if(eventlis) {
						eventlis.forEach((item) => {
							this.list.push(item)
						})
						uni.showToast({
							title: "事项获取成功,今天也是爱你的一天~",
							icon: "none"
						})
					}
				},
				fail(res) {
					uni.showToast({
						title: "哎呀事项获取失败了~",
						icon: "none"
					})
				}
			})
		},
		methods: {
			SwitchTab(index) {
				this.ActieIndex = index
				if(this.ActieIndex === 0) {
					this.tab_text = '全部'
				}
				if(this.ActieIndex === 1) {
					this.tab_text = '代办'
				}
				if(this.ActieIndex === 2) {
					this.tab_text = '已完成'
				}
			},
			handleStart(event) {
				this.lastX = event.changedTouches[0].pageX;
				this.lastY = event.changedTouches[0].pageY;
			},
			handleEnd(event) {
				let currentX = event.changedTouches[0].pageX;
				let currentY = event.changedTouches[0].pageY;
				if(Math.abs(currentY - this.lastY) < 30) {
					console.log(currentX)
					console.log(this.lastX)
					if(currentX - this.lastX > 5) {
						switch(this.ActieIndex) {
							case(0): this.ActieIndex = 2;break;
							case(1): this.ActieIndex = 0;break;
							case(2): this.ActieIndex = 1;break;
						}
					}
					else if(currentX - this.lastX < -5) {
						switch(this.ActieIndex) {
							case(0): this.ActieIndex = 1;break;
							case(1): this.ActieIndex = 2;break;
							case(2): this.ActieIndex = 0;break;
						}
					}
				}
				
			},
			handleClick() {
				if(this.$refs.addEvent.show) {
					this.$refs.addEvent.show = false
				}
			}
		}
	}
</script>

<style lang="stylus"> 
	// 状态栏样式
	.background
		height 100vh
		width 100vh
		position fixed
		background-size 100% 100%
		background-image url(../../static/sky.jpg)
		background-repeat no-repeat
		background-attachment fixed
	.header
		display: flex
		align-items: center
		justify-content: space-between
		position: fixed
		top: 0
		left: 0
		box-sizing border-box
		z-index 10
		width 100%
		font-size 14px
		padding 0 10px
		height 50px
		color #C0C0C0
		background #ffffff
		box-shadow -2px 2px 10px 2px #cccccc
		.header-left
			width 100%
		.header-right
			flex-shrink 0
			.header-right-content
				padding 0 5px
		.active-content
			padding-right 5px
			font-size 14px
			color  #52a6fe
			
</style>
