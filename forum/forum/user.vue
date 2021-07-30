<template>
	<div v-if="pageLoad" class="body">
		<div v-if="!unLogin" >
	
			<div class="main-body">
				<div class="flex flex-ai-center row-box mgb-5">

					<image @click="gourl('../../pages/user/user_head')" class="wh-60 bd-radius-50 mgr-10" :src="user.user_head+'.100x100.png'"></image>

					<div class=" flex-1">
						<div class="cl1 mgb-10">{{user.nickname}}</div>


						<div class="flex flex-ai-center">
							<div class="cl2 mgr-5">金币</div>
							<div class="cl-num mgr-10">{{user.gold}}</div>
							<div class="cl2 f12 mgr-5">帖子</div>
							<div class="cl2 f12 mgr-10 ">{{topic_num}}</div>
							<div class="cl2 f12">评论 {{comment_num}}</div>

						</div>
					</div>
					<navigator url="../../pages/user/set" class="flex-center btn-small btn-link iconfont icon-settings"></navigator>
				</div>

				<div class="row-box mgb-5">
					<div  @click="gourl('../../forum/forum/my')"  class="row-item">
						<div class="row-item-icon icon-news  cl-u"></div>
						<div class="row-item-title">我的贴子</div>
					</div>
					<div  @click="gourl('../../pages/pm/index')"  class="row-item">
						<div class="row-item-icon icon-message_light  cl-u"></div>
						<div class="row-item-title">我的私信</div>
					</div>
					
					<div  @click="gourl('../../forum/forum_comment/my')" class="row-item">
						<div class="row-item-icon icon-comment  cl-u"></div>
						<div class="row-item-title">我的评论</div>
					</div>
					<div  @click="gourl('../../forum/forum_fav/index')" class="row-item">
						<div class="row-item-icon icon-favor  cl-u"></div>
						<div class="row-item-title">我的收藏</div>
					</div>
					<div  @click="gourl('../../pages/gold_log/my')"  class="row-item">
						<div class="row-item-icon icon-choiceness  cl-u"></div>
						<div class="row-item-title">金币日志</div>
					</div>
					<div @click="gourl('../../pages/notice/my')" class="row-item">
						<div class="row-item-icon icon-notice  cl-u"></div>
						<div class="row-item-title">我的消息</div>
					</div>
				</div>

				<div class="row-box mgb-5">

					<div @click="gourl('../../pages/kefu/index')"  class="row-item">
						<div class="row-item-icon icon-service  cl-u"></div>
						<div class="row-item-title">联系客服</div>
					</div>
					<div @click="gourl('../../pages/html/aboutus')" class="row-item">
						<div class="row-item-icon icon-info  cl-u"></div>
						<div class="row-item-title">关于我们</div>
					</div>
				</div>

			</div>

		</div>
		<div v-else class="unLoginBox">
			<div class="flex flex-center mgb-20 cl2">您还未登录,请先登录</div>
			<div class="flex flex-center">
				<navigator class="btn-small" url="../../pages/login/index">前往登录</navigator>
			</div>

		</div>
		<mt-footer tab="user"></mt-footer>
	</div>
</template>

<script>
	import mtFooter from "../forumfooter.vue";
	export default {
		components: {
			mtFooter
		},
		data: function() {
			return {
				pageLoad: false,
				unLogin:false,
				user: {},
				topic_num:0,
				comment_num:0

			}
		},
		onLoad: function() {
			this.getPage();
		},
		onShow:function(){
			this.getPage();
		},
		onShareAppMessage: function() {

		},
		methods: {
			gourl:function(url){
				uni.navigateTo({
					url:url
				})
			},
			getPage: function() {
				var that = this;
				that.app.get({
					url: that.app.apiHost + "/forum/user",
					unLogin:true,
					success: function(res) {
						if(res.error){
							that.unLogin=true;
						}else{
							that.unLogin=false;
							that.user = res.user;
							that.topic_num=res.topic_num;
							that.comment_num=res.comment_num;
						}
						that.pageLoad = true;
						
					}
				})
			}
		},
	}
</script>

<style>
 

	.row-item-icon {
		font-size: 42rpx;
	}

	.cl-u,
	.cl-u:before {
		color: #ed6d53;

	}
</style>
