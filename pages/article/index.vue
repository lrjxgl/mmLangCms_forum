<template>
	<view>
		<view v-if="!pageLoad" class="">
			<page-loading></page-loading>
		</view>
		<view v-if="pageLoad">
			<scroll-view class="bg-fff" scroll-x="true">
				<view class="tabs-border">
					<view @click="setCat(0)"  :class="catid==0?'tabs-border-active':''" class="tabs-border-item-inner ">全部</view>
					<view 
					class="tabs-border-item-inner" 
					:class="catid==item.catid?'tabs-border-active':''" 
					v-for="(item,key) in  catList"
					 :key="key" @click="setCat(item.catid)">{{item.cname}}</view>
				</view>
			</scroll-view>

			<view class="main-body">
				<view class="flexlist">
					<view @click="goArticle(item.id)" class="flexlist-item pdb-10" v-for="(item,index) in  list" :key="index">

						<image v-if="item.imgurl!=''" class="flexlist-img" :src="item.imgurl+'.100x100.png'"></image>
						<view class="flex-1">
							<view class="flexlist-title f16">{{item.title}}</view>
							<view class="flexlist-desc cl2 f14">{{item.description}}</view>
						</view>

					</view>
				</view>


			</view>
		</view>
		<go-top></go-top>
	</view>
</template>

<script>
	import mtFooter from "../../components/footer.vue";
	export default{
		data:function(){
			return {
				pageLoad:false,
				list:[],
				per_page:0,
				isFirst:true,
				catList:[],
				catid:0
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
					url:that.app.apiHost+"/article/index",
					success:function(res){
						that.pageLoad=true;
						that.list=res.list;
						that.per_page=res.per_page;
						that.catList=res.catList;
					}
				})
			},
			getList:function() {
				var that=this;
				if(that.per_page==0 && !that.isFirst){
					return false;
				}
				that.app.get({
					url:that.app.apiHost+"/article/index",
					data:{
						per_page:that.per_page,
						catid:this.catid
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
			},
			goArticle:function(id){
				uni.navigateTo({
					url:"show?id="+id
				})
			},
			setCat:function(catid){
				this.catid=catid;
				this.per_page=0;
				this.isFirst=true;
				this.getList();
			}
		},
	} 
</script>

<style>

</style>
