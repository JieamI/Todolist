<template>
	<view class="login">
		<view class="content">
			<!-- 头部logo -->
			<view class="header">
				<image src="../../static/icon.png" mode="aspectFit"></image>
			</view>
			<!-- 主体表单 -->
			<view class="main">
				<wInput
					v-model="usr"
					type="text"
					placeholder="学号"
				></wInput>
				<wInput
					v-model="pwd"
					type="password"
					placeholder="密码"
				></wInput>
			</view>
			<wButton 
				text="登 录"
				:rotate="isRotate" 
				@click.native="startLogin()"
			></wButton>
		</view>
	</view>
</template>

<script>
	import wInput from '../../components/login/input.vue' //input
	import wButton from '../../components/login/button.vue' //button
	export default {
		data() {
			return {
				usr:'', 
				pwd:'',
				isRotate: false
			}
		},
		components:{
			wInput,
			wButton,
		},
		onLoad() {
			// 获取本地用户信息，实现免登录
			if(!getApp().globalData.logout) {
				try {
					if(uni.getStorageSync('UserData')[0] && uni.getStorageSync('UserData')[1]) {
						getApp().globalData.usr = uni.getStorageSync('UserData')[0]
						getApp().globalData.pwd = uni.getStorageSync('UserData')[1]
						uni.showToast({
							title: "本地缓存获取成功",
							icon: "none",
						})
						uni.redirectTo({
							url: '../index/index'
						})
					}
				}catch (e) {
					console.log(e)
					uni.showToast({
						title: "用户信息获取失败，请手动登录~",
						icon: "none"
					})
				}
			}
			getApp().globalData.logout = false
		},
		methods: {
			startLogin() {
				if(this.isRotate) {
					return
				}
				if(!this.usr || !this.pwd) {
				     uni.showToast({
				        icon: 'none',
				        title: '学号或密码不能为空'
				    });
				    return
				}
				this.isRotate=true
				var _this = this
				uni.request({
					url: getApp().globalData.url + 'login',
					method: 'POST',
					sslVerify: false,
					data: {
						usr: _this.usr,
						pwd: _this.pwd
					},
					success(res) {
						if(res.statusCode === 200 && res.data.status == 1) {
							getApp().globalData.usr = _this.usr
							getApp().globalData.pwd = _this.pwd
							try {
								uni.setStorageSync('UserData', [_this.usr, _this.pwd])
							}catch(e) {
								console.log(e)
							}
							setTimeout(() => {
								_this.isRotate = false
								uni.redirectTo({
									url: "../index/index"
								})
							}, 3000)
						}
						else {
							uni.showToast({
								title: '学号或密码错误~',
								icon: 'none'
							})
							_this.isRotate = false
						}
					},
					fail(res) {
						uni.showToast({
							title: '登录失败',
							icon: 'none'
						})
						console.log(res)
						_this.isRotate = false
					}
				})
			}
		}
	}
</script>

<style>
	@import url("../../common/main.css");
</style>
