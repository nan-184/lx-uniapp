<template>
	<view class="content">

		<u-select v-model="show" model="single-column" :list="phone"></u-select>
		<!-- 下一步-->
		<u-button :disabled="agreebtn" class="register-btn" type="primary">下一步</u-button>
		<view class="reg-but">
			<view class="register-title">注册</view>

		</view>
		<form class="form-form">
			<!-- 手机号 -->
			<view class="phone-btn">
				<view @blur="regPh" @click="selectphone" class="phone-code">+86</view>
				<input :disabled="disPhone" class="input-ph" type="text" placeholder="请输入手机号" v-model="value">
			</view>

			<!-- 验证码 -->
			<view class="id-code">
				<input class="input-ph" type="password" placeholder="请输入验证码">
				<view class="wrap">
					<u-toast ref="uToast"></u-toast>
					<u-verification-code :seconds="seconds" @end="end" @start="start" ref="uCode" @change="codeChange">
					</u-verification-code>
					<view @tap="getCode" class="get-id-code">{{tips}}</view>
				</view>
			</view>

			<!-- 密码 -->
			<view class="pwd">
				<input class="input-ph" :type="type" placeholder="请输入密码">
				<view v-if="glass" @click="pwd" class="iconfont">&#xe630;</view>
				<view v-else @click="pwd" class="iconfont">&#xe663;</view>
			</view>
			<!-- 确认密码 -->
			<view class="pwd-d">
				<input class="input-ph " :type="type1" placeholder="请输入确认密码">
				
				<view v-if="glass1" @click="pwd1" class="iconfont">&#xe630;</view>
				<view v-else @click="pwd1" class="iconfont">&#xe663;</view>
			</view>
		</form>
		<checkbox @click="agreeCheck">我已阅读并同意</checkbox>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				glass:true,
				glass1:true,
				value: "",
				type: 'password',
				type1: 'password',
				show: false,
				disPhone: false,
				agreebtn: true,
				tips: '',
				seconds: 5,
				phone: [{
						value: '1',
						label: '+86'
					},
					{
						value: '2',
						label: '+852'
					},
					{
						value: '3',
						label: '+853'
					}
				]
			}
		},
		onLoad() {},
		methods: {
			//验证手机号格式
			regPh() {
				let reg = /^1[3-9]\d{9}$/;
				if (value.reg){
					console.log("√")
				}else{
					console.log("×")
				}
			},
			//是否阅读同意
			agreeCheck() {
				if (this.agreebtn) {
					this.agreebtn = false
				} else {
					this.agreebtn = true
				}
			},
			//获取验证码
			codeChange(text) {
				this.tips = text;
			},
			getCode() {
				if (this.$refs.uCode.canGetCode) {
					// 模拟向后端请求验证码
					uni.showLoading({
						title: '正在获取验证码'
					})
					setTimeout(() => {
						uni.hideLoading();
						// 这里此提示会被this.start()方法中的提示覆盖
						this.$u.toast('验证码已发送');
						// 通知验证码组件内部开始倒计时
						this.$refs.uCode.start();
					}, 2000);
				} else {
					this.$u.toast('倒计时结束后再发送');
				}
			},
			end() {
				this.$u.toast('验证码已失效，可重新点击发送');
				this.disPhone = false
			},
			start() {
				this.$u.toast('验证码已发送');
				this.disPhone = true
			},
			//选择电话区号
			selectphone() {
				this.show = true
			},
			// 控制密码的显示和隐藏
			pwd() {
				
				if (this.type == "password") {
					this.type = "text";
					this.glass=false
				} else {
					this.type = "password";
					this.glass=true
				}
			},
			pwd1() {
				if (this.type1 == "password") {
					this.type1 = "text";
					this.glass1=false
				} else {
					this.type1 = "password";
					this.glass1=true
				}
			}



		}

	}
</script>

<style lang="scss">
	@import "uview-ui/index.scss";

	.content {

		//下一步
		.register-btn {
			width: 150rpx;
			height: 75rpx;
			margin: 0 0 0 600rpx;
			border-radius: 20rpx;
		}

		.reg-but {
			display: flex;
			flex-direction: row;
			justify-content: space-around;
			align-items: center;

			//注册
			.register-title {
				font-size: 40rpx;
				text-align: center;
			}
		}

		.form-form {
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;

			//手机号
			.phone-btn {
				display: flex;
				flex-direction: row;
				border-bottom: 1px solid #C0C0C0;

				.phone-code {
					font-size: 30rpx;
					text-align: center;
					padding: 30rpx 20rpx 0 0;
					// border: 1px solid #C0C0C0;
				}

				.input-ph {
					height: 100rpx;
					width: 500rpx;

				}

			}

			//验证码
			.id-code {
				display: flex;
				flex-direction: row;
				font-size: 20rpx;
				border-bottom: 1px solid #C0C0C0;

				.input-ph {
					height: 100rpx;
					width: 500rpx;

				}

				.wrap {
					.get-id-code {
						padding-top: 15rpx;
						width: 150rpx;
						// border: 1px solid #000000;
						font-size: 30rpx;
						text-align: center;
					}
				}
			}

			.pwd {
				display: flex;
				flex-direction: row;

			}

			.pwd-d {
				display: flex;
				flex-direction: row;
			}

			.input-ph {
				height: 100rpx;
				width: 500rpx;
				border-bottom: 1px solid #C0C0C0;
			}



		}
	}
</style>
