<template>
	<view class="main-list oBorder">
		<!-- 文本框 -->
		<input 
			class="main-input" 
			:value="value" 
			:type="_type" 
			:placeholder="placeholder" 
			:password="type==='password'&&!showPassword" 
			@input="onInput"
		/>
		<!-- 是否可见密码 -->
		<image 
			v-if="_isShowPass&&type==='password'&&!_isShowCode"
			class="img cuIcon" 
			:class="showPassword?'cuIcon-attention':'cuIcon-attentionforbid'" 
			@tap="showPass"
		></image>
	</view>
</template>

<script>
	var _this
	export default{
		data(){
			return{
				showPassword: false, //是否显示明文
			}
		},
		props:{
			type: String, //类型
			value: String, //值
			placeholder: String, //框内提示
			isShowPass:{
				//是否显示密码图标（二选一）
				type: [Boolean,String],
				default: false,
			}
		},
		model: {
			prop: 'value',
			event: 'input'
		},
		methods:{
			showPass(){
				//是否显示密码
				this.showPassword = !this.showPassword
			},
			onInput(e) {
				//传出值
				this.$emit('input', e.target.value)
			}
		},
		computed:{
			_type(){
				//处理值
				const type = this.type
				return type == 'password' ? 'text' : type
			},
			_isShowPass() {
				//处理值
				return String(this.isShowPass) !== 'false'
			},
			_isShowCode() {
				//处理值
				return String(this.isShowCode) !== 'false'
			}
		}
	}
</script>

<style>
	@import url("../../common/icon.css");
	
	.main-list{
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		height: 36upx;   /* Input 高度 */
		color: #333333;
		padding: 32upx;
		margin-top:24upx;
		margin-bottom: 24upx;
	}
	.img{
		width: 32upx;
		height: 32upx;
		font-size: 32upx;
	}
	.main-input{
		flex: 1;
		text-align: left;
		font-size: 28upx;
	}
	.vercode {
		color: rgba(0,0,0,0.7);
		font-size: 24upx;
		line-height: 100upx;
	}
	.oBorder{
	    border: none;
	    border-radius: 2.5rem ;
	    -webkit-box-shadow: 0 0 60upx 0 rgba(43,86,112,.1) ;
	    box-shadow: 0 0 60upx 0 rgba(43,86,112,.1) ;
	}
</style>
