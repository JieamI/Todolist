<template>
	<view>
		<view class="header">
			<view class="header-day" v-for="(item, index) in header_day" :key="index">{{item}}</view>
		</view>
		<view class="wrap">
			<view class="column">
				<view class="content Mon" :class="{'no-content': !item.status}" v-for="(item, index) in course['Mon']" :key="item.id">
					<text>{{item.content[0]}}</text>
					<text>{{item.content[1]}}</text>
					<view class="lone-term-course" v-if="item.duration ===  1"></view>
				</view>
			</view>
			<view class="column">
				<view class="content Tue" :class="{'no-content': !item.status}" v-for="(item, index) in course['Tue']" :key="item.id">
					<text>{{item.content[0]}}</text>
					<text>{{item.content[1]}}</text>
					<view class="lone-term-course" v-if="item.duration ===  1"></view>
				</view>
			</view>
			<view class="column">
				<view class="content Wed" :class="{'no-content': !item.status}" v-for="(item, index) in course['Wed']" :key="item.id">
					<text>{{item.content[0]}}</text>
					<text>{{item.content[1]}}</text>
					<view class="lone-term-course" v-if="item.duration ===  1"></view>
				</view>
			</view>
			<view class="column">
				<view class="content Thu" :class="{'no-content': !item.status}" v-for="(item, index) in course['Thu']" :key="item.id">
					<text>{{item.content[0]}}</text>
					<text>{{item.content[1]}}</text>
					<view class="lone-term-course" v-if="item.duration ===  1"></view>
				</view>
			</view>
			<view class="column">
				<view class="content Fri" :class="{'no-content': !item.status}" v-for="(item, index) in course['Fri']" :key="item.id">
					<text>{{item.content[0]}}</text>
					<text>{{item.content[1]}}</text>
					<view class="lone-term-course" v-if="item.duration ===  1"></view>
				</view>
			</view>
			<view class="column">
				<view class="content Sat" :class="{'no-content': !item.status}" v-for="(item, index) in course['Sat']" :key="item.id">
					<text>{{item.content[0]}}</text>
					<text>{{item.content[1]}}</text>
					<view class="lone-term-course" v-if="item.duration ===  1"></view>
				</view>
			</view>
			<view class="column">
				<view class="content Sun" :class="{'no-content': !item.status}" v-for="(item, index) in course['Sun']" :key="item.id">
					<text>{{item.content[0]}}</text>
					<text>{{item.content[1]}}</text>
					<view class="lone-term-course" v-if="item.duration ===  1"></view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				course: {},
				header_day: ['周一', '周二', '周三', '周四', '周五', '周六', '周日']
			}
		},
		onLoad() {
			for(let key in getApp().globalData.course) {
				this.course[key] = []
				var cday = getApp().globalData.course[key]
				cday.forEach((item, index)=>{
					if(item) {
						let num = item.split('◇')[1].match(/\(\d+-\d+节\)/)[0].replace('(','').replace(')', '').replace('节', '')
						this.course[key].push({
							content: item.split('◇')[0].split('@'),
							status: true,
							id: key + index,			//number和string运算自动转换为string
							duration: Number(num.split('-')[1]) - Number(num.split('-')[0])
						})
					}
					else {
						this.course[key].push({
							content: ['', ''],
							status: false,
							id: key + index,
							duration: 0,
						})
					}
				})
			}
		},
		onPullDownRefresh() {
			// 关闭当前页面并重定向回首页
			uni.navigateBack()		
			uni.stopPullDownRefresh()
		},
		methods: {
			
		}
	}
</script>

<style lang="stylus" scoped>
	.header
		display flex
		.header-day
			flex 1
			display flex
			justify-content center
			margin 0 3px
			font-size 16px
			font-weight 600
			color #C8C7CC
	.wrap
		display flex
		height 100vh
		.column
			display flex
			flex-direction column
			align-items center
			flex 1
			overflow hidden
			margin 0 3px
			justify-content space-around
			.content
				display flex
				flex-direction column
				justify-content space-between
				align-items center
				width 100%
				height 100%
				margin 2px 0
				padding 3px
				font-size 10px
				border-radius 14px
				background #4CD964
				color #FFFFFF
				.lone-term-course
					background #FFFFFF
					position relative
					top 4.5px
					border-radius 0 0 8px 8px
					width 100%
					height 30%
				.lone-term-course:after
					position absolute
					content ''
					border-radius 0 0 8px 8px
					top -4.5px
					width 100%
					height 30%
			.Mon 
				background #F0AD4E
				.lone-term-course:after
					background #F0AD4E
			.Tue
				background #999999
				.lone-term-course:after
					background #999999
			.Wed
				background #52A6FE
				.lone-term-course:after
					background #52A6FE
			.Thu
				background #4CD964
				.lone-term-course:after
					background #4CD964
			.Fri
				background #DD524D
				.lone-term-course:after
					background #DD524D
			.Sat
				background #DD524D
				.lone-term-course:after
					background #DD524D
			.Sun
				background #43c8dc
				.lone-term-course:after
					background #43c8dc
			.no-content
				background #FFFFFF
	
</style>
