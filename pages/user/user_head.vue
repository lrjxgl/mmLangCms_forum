<template>
	<view>
		<view v-if="pageLoad">
			<view class="flex flex-center mgt-10 mgb-10">
				<image @click="upload()" mode="widthFix" class="wh-200 pointer" :src="user_head"></image>
			</view>
		</view>
	</view>
</template>

<script>
	 
	export default{
		data:function(){
			return {
				pageLoad:false, 
				 
				user_head:"",
			}
		},
		onLoad:function(option){
			 
			this.getPage();
		},
		 
		methods:{
			getPage:function(){
				var that=this;
				that.app.get({
					url:that.app.apiHost+"/user/head",
					success:function(res){
						that.pageLoad=true;
						 
						that.user_head=res.user.true_user_head; 
					}
				})
			},
			upload:function(){
				var that=this;
				uni.chooseImage({
					success: (chooseImageRes) => {
						const tempFilePaths = chooseImageRes.tempFilePaths;
						uni.uploadFile({
							url: that.app.apiHost+"/upload/img?token="+that.app.getToken(), //仅为示例，非真实的接口地址
							filePath: tempFilePaths[0],
							name: 'upimg',
							
							success: function(res){
								var data=JSON.parse(res.data);
								if(data.error){
									uni.showToast({
										title:data.message,
										icon:"none"
									})
									return false;
								}
								that.app.post({
									url:that.app.apiHost+"/user/headsave",
									data:{
										user_head:data.imgurl
									},
				
									success:function(res){
										that.user_head=res.data.trueimgurl;
									}
								})
							}
						});
					}
				});
			}
		},
	}
</script>

<style>
</style>
