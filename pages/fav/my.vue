<template>
	<view v-if="pageLoad">
		<view class="tabs-border">
			<view @click="setTable('article')" :class="tablename=='article'?'tabs-border-active':''" class="tabs-border-item">文章</view>
			<view @click="setTable('forum')" :class="tablename=='forum'?'tabs-border-active':''"  class="tabs-border-item">帖子</view>
			 
		</view>
		<view v-if=" list.length==0">
			<view class="emptyData">暂无收藏</view>
		</view>
		<view v-else>
			<view v-for="(item,key) in list" :key="key">
				<view v-if="tablename=='article'">
					<view @click="goArticle(item.id)" class="row-item bg-fff">
						<view class="flex-1">
						<view class="cl1 f18">{{item.title}}</view> 
						<view class="cl2">{{item.description}}</view>
						</view>
					</view>
				</view>	
				 
				<view v-else-if="tablename=='forum'">
					<view @click="goForum(item.id)" class="row-item bg-fff">
						<view class="flex-1">
						<view class="cl1 f18">{{item.title}}</view> 
						 
						</view>
					</view>
				</view> 
			</view>
		</view>

	</view>
</template>

<script>
 
	export default {

		data: function() {
			return {
				pageLoad: false,
				list: [],
				per_page:0,
				isFirst:true,
				tablename: "article"
			}
		},
		onLoad: function(option) {

			uni.setNavigationBarTitle({
				title: '我的收藏'
			});
			this.getPage();
		},
		onReachBottom: function() {
			this.getList();
		},
		onPullDownRefresh: function() {
			this.refresh();
		},
		methods: {
			getPage: function() {
				var that = this;
				that.app.get({
					url: that.app.apiHost + "/fav/my",
					data: {
	
						tablename: that.tablename
					},
					success: function(res) {
						that.isFirst = false;
						that.list = res.list;
						that.pageLoad = true;
						that.per_page = res.per_page;
					}
				})
			},

			getList: function() {
				var that = this;
				if (!that.isFirst && that.per_page == 0) return false;
				that.app.get({
					url: that.app.apiHost + "/fav/my",
					data: {
						per_page:that.per_page,
						tablename: that.tablename
					},
					success: function(res) {
						if(that.isFirst){
							that.isFirst = false;
							that.list = res.list;
						}else{
							for(var i in res.list){
								that.list.push(res.list[i]);
							}
						}
						that.pageLoad = true;
						that.per_page = res.per_page;
					}
				}) 
			},
		 
			goForum: function(id) {
				uni.navigateTo({
					url: "../forum/show?id=" + id
				})
			},
			goArticle: function(id) {
				uni.navigateTo({
					url: "../article/show?id=" + id
				})
			},
			refresh: function() {
				this.getPage();
				setTimeout(function() {
					uni.stopPullDownRefresh();
				}, 1000)
			},
			loadMore: function() {
				this.getList();
			},
			setTable:function(tablename){
				this.tablename=tablename;
				this.isFirst=true;
				this.per_page=0;
				this.getList();
			}
		},
	}
</script>

<style>


</style>
