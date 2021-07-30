<template>
	<view>
		<form v-if="user.telephone==''" @submit="submit()">
			<div class="input-flex">
				<label class="input-flex-label">手机</label>
				<input class="input-flex-text" v-model="telephone" name="telephone" id="telephone" />
				
			</div>
			<view class="input-flex">
				<view class="input-flex-label">验证码</view>
				<input type="text" class="input-flex-text" name="yzm" v-model="yzm">
				<view class="input-flex-btn" v-bind:class="yzmClass" @click="getYzm()">{{yzmStatus}}</view>
			</view>
			
			<button form-type="submit" class="btn-row-submit">绑定手机</button>
		</form>
		<div v-else class="emptyData">
			您绑定的手机:{{user.telephone}}
		</div>
	</view>
</template>

<script>
	var yzmTimer=60,yzmTime=59,yzmEnable=true;
	export default {
		data: function() {
			return {
				pageLoad: false,
				yzm: "",
				telephone: "",
				yzmClass: "",
				yzmStatus: "获取验证码",
				user:{}
			}
		},
		onLoad: function(option) {
			uni.setNavigationBarTitle({
				title: "支付密码"
			})
			this.getPage();
		},

		methods: {
			downTimer: function() {
				var that = this;
				var it = setInterval(function() {
					yzmEnable = false;
					that.yzmStatus = "倒计时" + yzmTime + "秒";
					that.yzmClass = "yzmDisable";
					yzmTime--;
					if (yzmTime == 0) {
						yzmTime = 59;
						yzmEnable = true;
						that.yzmClass = "";
						that.yzmStatus = "获取验证码";
						clearInterval(it);
					}
				}, 1000);
			},
			getPage:function(){
				var that=this;
				that.app.get({
					url:that.app.apiHost+"/user/info",
					 
					success:function(res){
						that.user=res.user;
					}
				})
			},
			getYzm: function() {
				if (!yzmEnable) return false;
				var that = this;
				that.app.get({
					url: that.app.apiHost + "/user/sendsms",
					data:{
						telephone:this.telephone
					},
					success: function(res) {
						uni.showToast({
							title: res.message,
						})
						return false;
						if (!res.error) {
							that.downTimer();
						}

					}
				})
			},
			submit: function(e) {
				var that = this;
				that.app.post({
					url: this.app.apiHost + "/user/telephonesave",
					data: {
						telephone:this.telephone,
						yzm:this.yzm
					},
					success: function(res) {
						uni.showToast({
							"title": res.message
						})
						if (!res.error) {
							setTimeout(function() {
								uni.navigateBack()
							}, 600)

						}

					}
				})
			}
		},
	}
</script>

<style>
</style>
