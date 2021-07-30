<template>
	<view v-if="pageLoad" class="bg-a">

		<view v-if="rscount==0">
			<view class="emptyData">暂无消息</view>
		</view>
		<view v-else>
			<view class="row-box  mgb-5" v-for="(item,index) in list" :key="index">
				<view class="flex bd-mp-10">
					<view class="btn-mini btn-outline-success">{{item.status_name}}</view>
					<view class="flex-1"></view>
					<view class="cl3 f12">{{item.timeago}}</view>
				</view>

				<view>
					<rich-text :nodes="item.content"></rich-text>
				</view>

			</view>
		</view>


	</view>
</template>

<script>
	export default{
		data:function(){
			return {
				pageLoad:false,
				list:[],
				per_page:0,
				isFirst:true,
				rscount:0
			}
		},
		onLoad:function(){
			this.getPage();
		},
		onReachBottom:function(){
			this.getList();
		},
		onPullDownRefresh:function(){
			this.getPage();
			uni.stopPullDownRefresh();
		},
		onShareAppMessage:function(){
			
		},
		onShareTimeline:function(){
			
		},
		methods: {
			gourl:function(url){
				uni.navigateTo({
					url:url
				})
			},
			getPage:function() {
				var that=this;
				that.app.get({
					url:that.app.apiHost+"/notice/my",
					success:function(res){
						that.pageLoad=true;
						that.list=res.list;
						that.rscount=res.rscount;
						that.per_page=res.per_page;
					}
				})
			},
			getList:function() {
				var that=this;
				if(that.per_page==0 && !that.isFirst){
					return false;
				}
				that.app.get({
					url:that.app.apiHost+"/notice/my",
					data:{
						per_page:that.per_page
					},
					success:function(res){						 
						that.per_page=res.per_page;
						if(that.isFirst){
							that.list=res.list;
							that.isFirst=false;
						}else{
							for(var i in res.list){
								that.list.push(res.list[i]);
							}							
						}
						
					}
				})
			}
		},
	}
</script>

<style>

</style>
