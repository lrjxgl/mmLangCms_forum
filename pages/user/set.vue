<template>
	<view>
		<view v-if="pageLoad">
			<view class="row-box">
				 
				<view class="row-item"  @click="gourl('../user/info')">
					<view class="row-item-icon icon-people"></view>
					<view class="flex-1"> {{user.nickname}}</view>
					 
				</view>
				<view class="row-item"  @click="gourl('../user/telephone')">
					<view class="row-item-icon icon-mobile"></view>
					<view v-if="user.telephone!=''" class="flex-1"> {{user.telephone}}</view>
					<view v-else class="flex-1">去绑定</view> 
				</view>
				<view class="row-item" @click="gourl('../user/password')" >
					<view class="row-item-icon icon-password"></view>
					<view class="flex-1">登录密码</view>
				</view>
				<view class="row-item" @click="gourl('../user/paypwd')" >
					<view class="row-item-icon icon-password"></view>
					<view class="flex-1">支付密码</view>
				</view> 
			</view>
			 
			 <view class="btn-row-submit bg-danger" @click="loginOut()">注销</view>
		</view>
	</view>
</template>

<script>
	 
	export default{
		data:function(){
			return {
				pageLoad:false, 
				user:{}
			}
		},
		onLoad:function(option){
		 
			this.getPage();
		},
		 
		methods:{
			getPage:function(){
				var that=this;
				that.app.get({
					url:that.app.apiHost+"/user/info",
					 
					success:function(res){
						if(res.error){
							that.app.goHome();
						}else{
							that.pageLoad=true;
							that.user=res.data.user;
						}
						
						 
					}
				})
			},
			gourl:function(url){
				uni.navigateTo({
					url:url,
				})
			},
			loginOut:function(){
				var that=this; 
				that.app.get({
					url:that.app.apiHost+"/login/logout",
					data:{
						iRefresh_token:uni.getStorageSync("refresh_token"),
						iToken:uni.getStorageSync("token")
					},
					success:function(res){
						if(res.error){
							return false;
						}
						uni.removeStorageSync("token");
						uni.removeStorageSync("token_expire");
						uni.removeStorageSync("refresh_token");
						uni.removeStorageSync("refresh_token_expire");
						uni.showToast({
							"title": res.message
						}); 
						uni.showToast({
							title:"退出登录中...",
							icon:"none"
						})
						setTimeout(function(){
							that.app.goHome();	
						},1000)
						
					}
				})
			} 
		},
	}
</script>

<style>
</style>
