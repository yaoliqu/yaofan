<template>
	<view class="container">
		<view class="login-wrapper">
			<view class="header">Login</view>
			<view class="form-wrapper">
				<input type="text" name="username" placeholder="username" class="input-item" v-model="username" />
				<input type="password" name="password" placeholder="password" class="input-item" v-model="password" />
				<view class="btn" @click="Login">登录</view>
			</view>
			<view class="msg">不知道账号密码?<a href="#" @click="open">点这里</a></view>
			<uni-popup ref="popup" type="dialog">
				<uni-popup-dialog type="info" class="login_dialog" content="boy:大辣鸡/20171121
				 girl:大傻子/20171121" @confirm="confirm"></uni-popup-dialog>
			</uni-popup>
		</view>
	</view>
</template>

<script>
	import uniPopup from '@/components/uni-popup/uni-popup.vue'
	import uniPopupMessage from '@/components/uni-popup/uni-popup-message.vue'
	import uniPopupDialog from '@/components/uni-popup/uni-popup-dialog.vue'
	export default {
		data() {
			return {
				username: "",
				password: ""
			}
		},
		components: {
			uniPopup,
			uniPopupMessage,
			uniPopupDialog
		},
		methods: {
			open() {
				this.$refs.popup.open()
			},
			confirm(done, value) {
				console.log(value)
				// TODO 做一些其他的事情，手动执行 done 才会关闭对话框
				done()
			},
			Login() {
				if (this.username == "大傻子" && this.password == '20171121') {
					uni.setStorage({
						key: 'username',
						data: this.username,
						success: function() { 
							uni.switchTab({
								url: '/pages/index/index'
							});
						}
					});
				} else {
					uni.showToast({
						title: '名称或密码错误',
						duration: 2000,
						icon:"none"
					});
				}
			}
		}
	}
</script>

<style>
	page {
		margin: 0;
		padding: 0;
		font-family: 'Open Sans Light';
		letter-spacing: 10upx;
		height: 100%;
	}

	.container {
		height: 100%;
		background-image: linear-gradient(to right, #fbc2eb, #a6c1ee);
	}

	.login-wrapper {
		background-color: #fff;
		width: 400upx;
		height: 900upx;
		border-radius: 30upx;
		padding: 0 100upx;
		position: relative;
		left: 50%;
		top: 50%;
		transform: translate(-50%, -50%);
	}

	.login-wrapper .header {
		font-size: 60upx;
		font-weight: bold;
		text-align: center;
		line-height: 350upx;
	}

	.login-wrapper .form-wrapper .input-item {
		display: block;
		width: 100%;
		margin-bottom: 40upx;
		border: 0;
		padding: 20upx;
		border-bottom: 2upx solid rgb(128, 125, 125);
		font-size: 30upx;
		outline: none;
	}

	.login-wrapper .form-wrapper .input-item::placeholder {
		text-transform: uppercase;
	}

	.login-wrapper .form-wrapper .btn {
		text-align: center;
		padding: 20upx;
		width: 100%;
		margin-top: 80upx;
		background-image: linear-gradient(to right, #a6c1ee, #fbc2eb);
		color: #fff;
		border-radius: 20upx;
	}

	.login-wrapper .msg {
		text-align: center;
		line-height: 100upx;
		font-size: 24upx;
		color: #808080;
	}

	.login-wrapper .msg a {
		text-decoration-line: none;
		color: #a6c1ee;
	}

	.login_dialog {
		letter-spacing: 5upx;
	}
</style>
