<template>
	<view>
		<image src="../../static/logo-1.png" mode="widthFix" class="logo"></image>
		<view class="logo-title">EMOS企业在线办公系统</view>
		<view class="logo-subtitle">Ver 2050.2</view>
		<button class="login-btn" open-type="getUserInfo" @tap="login()">登陆系统</button>
		<view class="register-container">
			没有账号？
			<text class="register" @tap="toRegister()">立即注册</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {};
		},
		methods: {
			toRegister: function() {
				uni.navigateTo({
					url: '../register/register'
				});
			},
			login: function() {
				let that = this;
				uni.login({
					provider: 'weixin',
					success: function(resp) {
						let code = resp.code;
						that.ajax(that.url.login, "POST", {
							"code": code
						}, function(resp) {
							let permission = resp.data.permission;
							uni.setStorageSync('permission', permission);
							//console.log(permission);

							// 跳转到登陆页面
							uni.switchTab({
								url: '../index/index'
							});
						})
						console.log("success");
					},
					fail: function(e) {
						console.log(e)
						uni.showToast({
							icon: "none",
							title: "执行异常"
						})
					}
				});
			}
		}
	}
</script>

<style lang="less">
	@import url('login.less');
</style>
