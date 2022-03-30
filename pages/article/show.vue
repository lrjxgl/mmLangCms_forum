<template>
	<view>
		<view v-if="errMsg!=''" class="emptyData">文章出走啦</view>
		<view class="pd-10 bg-fff" v-if="pageLoad">
			<view class="d-title">{{data.title}}</view>
			<div class="flex mgb-10">
				<div class="f12 cl3">{{data.createtime}}</div>
			</div> 
			<view class="d-content">
			 
				<jyf-parser class="wmax" :html="data.content" ref="article"></jyf-parser> 
			</view>
			<view class="flex flex-center mgb-10">
				<love dtable="article" :dobjectid="data.id"></love>
				<fav dtable="article" :dobjectid="data.id"></fav> 
				<button style="margin: 0;" open-type="share" class="btn-share " @click="share()">分享</button>
			</view>
			<view class="comment-hd">回复列表</view>
			<cmform tablename="article" :objectid="data.id"></cmform>
			<view v-if="skyShareShow">
				<sky-share :title="data.title" :imgurl="data.imgurl" :link="shareLink" :desc="data.description"  :isshow="1"></sky-share>
			</view>
		</view>
	</view>
</template>

<script> 
	import cmform from "../../components/cmform.vue";
	var id;
	
	import skyShare from "../../components/skyshare.vue";
	import jyfParser from "../../components/jyf-parser/jyf-parser"; 
	import love from "../../components/love.vue";
	import fav from "../../components/fav.vue";
	export default{
		components: {
			skyShare,
			cmform,
			 jyfParser,
			love,
			fav
		},
		data:function(){
			return {
				pageLoad:false, 
				data:{},
				skyShareShow:0,
				shareLink:"",
				errMsg:""
			}
		},
		onLoad:function(option){
			id=option.id;
			this.getPage();
			 
		},
		onShareAppMessage:function(){
			
		},
		 onShareTimeline:function(){
		 	
		 },
		methods:{
			share:function(){
				var that=this;
				this.skyShareShow=0;
				setTimeout(function(){
					that.skyShareShow=1;
				},30)
			},
			getPage:function(){
				var that=this;
				that.app.get({
					url:that.app.apiHost+"/article/show?id="+id,
				 
					success:function(res){
						if(res.error){
							that.errMsg=res.message;
							return false;
						}
						that.pageLoad=true;
						that.data=res.data.data;
						that.shareLink=that.app.appRoot+"/article/show?id="+res.data.data.id;
						uni.setNavigationBarTitle({
							title:res.data.data.title
						})
					}
				})
			}
		},
	}
</script>

<style>
</style>