<template>  
    <view class="container">  
		<!-- <hx-navbar
			:status-bar="true"
			:back="false"
			barPlaceholder="hidden"
			:fixed="true"
			:title.sync="pageTitle"
			:color="['#ffffff','#666666']"
			transparent="auto"
			:background-color="[255,255,255]"
			@click-left="navTo(userSettingUrl)">
			<view  slot="left" >
				<view style="margin-left: 26upx;font-size: 22px;">
						<i class="hxicon-settings"></i>
				</view>
			</view>
			<view  slot="right" >
				<view style="margin-right: 16upx;font-size: 22px;">
						<i class="hxicon-mark"></i>
				</view>
			</view>
		</hx-navbar> -->

		<view class="user-section ">
			<view class="user-info-box">
				<view class="portrait-box" @click="userBash">
					<image class="portrait" :src="memberInfo.face || defaultFace"></image>
				</view>
				<view class="info-box" @click="userBash">
					<text class="username">{{memberInfo.nickname || '登录/注册'}}</text>
				</view>
			</view>
			<view class="vip-card-box">
				<image class="card-bg" src="/static/img/user/vip-card-bg.png" mode=""></image>
				<view class="b-btn">
					立即开通
				</view>
				<view class="tit">
					<text class="hxicon-iLinkapp-"></text>
					DCloud会员
				</view>
				<text class="e-m">DCloud Union</text>
				<text class="e-b">开通会员开发无bug 一测就上线</text>
			</view>
		</view>
		
		<view 
			class="cover-container"
			:style="[{
				transform: coverTransform,
				transition: coverTransition
			}]"
			@touchstart="coverTouchstart"
			@touchmove="coverTouchmove"
			@touchend="coverTouchend"
		>
			<image class="arc" src="/static/img/user/arc.png"></image>
			
			<!-- 订单 -->
			<view class="order-section">
				<navigator  class="order-item" url="/pages/order/order?state=1" open-type="switchTab"  hover-class="common-hover"  :hover-stay-time="50">
					<text class="hxicon-pay"></text>
					<text>正在团</text>
				</navigator >
				<navigator  class="order-item" url="/pages/order/order?state=2" open-type="switchTab"   hover-class="common-hover" :hover-stay-time="50">
					<text class="hxicon-deliver"></text>
					<text>正在发</text>
				</navigator >
				<navigator  class="order-item" url="/pages/order/order?state=3" open-type="switchTab"  hover-class="common-hover"  :hover-stay-time="50">
					<text class="hxicon-comment"></text>
					<text>已完成</text>
				</navigator >
			</view>
			<view class="history-section icon">
				<list-cell icon="hxicon-moneybagfill" iconColor="#e07472" title="收货地址" tips="000"></list-cell>
				<list-cell icon="hxicon-locationfill" iconColor="#5fcda2" title="会员" @eventClick="navTo('/pages/user/address/address')"></list-cell>
				<list-cell icon="hxicon-round_pay_fill" iconColor="#9789f7" title="系统设置" tips="000"></list-cell>
				<list-cell icon="hxicon-message_fill_light" iconColor="#ee883b" title="客服" tips="。。"></list-cell>
				<list-cell icon="hxicon-round_favor_fill" iconColor="#54b4ef" title="帮助"></list-cell>
				<!-- <list-cell icon="hxicon-settings" iconColor="#e07472" title="设置" border="" @eventClick="navTo('/pages/user/setting/setting')"></list-cell> -->
			</view>
		</view>
		<view class="uni-container-tabbar">
			<view class="uni-container-tabbar-container">
				<view v-for="(tab, tabIndex) in tabbarData" :key="tabIndex" class="uni-container-tabbar-img" @click="gotabbar(tabIndex)">
					<img :src="tab.iconPath" />
				</view>
			</view>
		</view>
		
    </view>  
</template>  
<script>  
	import hxNavbar from "@/components/hx-navbar/hx-navbar.vue"

	import listCell from '@/components/mix-list-cell';
    import {  
        mapState 
    } from 'vuex';
	let startY = 0, moveY = 0, pageAtTop = true;

	const TABBAR = [{
		"pagePath": "pages/tabBar/home/home",
		"iconPath": "/static/orange/home.png",
	},
	{
		"pagePath": "pages/tabBar/msg/msg",
		"iconPath": "/static/msg_grey@3x.png",
	},
	{
		"pagePath": "pages/tabBar/user/user",
		"iconPath": "/static/me_grey@3x.png",
	}]
    export default {
		components: {
			listCell,
			hxNavbar
		},
		data(){
			return {
				pageTitle:"我的",
				coverTransform: 'translateY(0px)',
				coverTransition: '0s',
				moving: false,
				defaultFace: '/static/img/user/missing-face.png',
				userSettingUrl: '/pages/user/setting/setting',
				
				memberInfo: {
					face: '',
					nickname: "",
				},
				tabbarData: TABBAR
			}
		},
		onLoad(){
			
			
		},
		onReady() {
			
		},
		onShow(){

			// let member = this.userData.member;
			// if(member){
			// 	this.memberInfo = {
			// 		face: member.facke, 
			// 		nickname: member.nickname
			// 	}
			// }else{
			// 	this.memberInfo = {
			// 		face: '', 
			// 		nickname: ''
			// 	}
			// }
			
		},
		// #ifndef MP
		onNavigationBarButtonTap(e) {
			const index = e.index;
			if (index === 0) {
				this.navTo('/pages/user/setting/setting');
			}else if(index === 1){
				// #ifdef APP-PLUS
				const pages = getCurrentPages();
				const page = pages[pages.length - 1];
				const currentWebview = page.$getAppWebview();
				currentWebview.hideTitleNViewButtonRedDot({
					index
				});
				// #endif
				uni.navigateTo({
					url: '/pages/notice/notice'
				})
			}
		},
		// #endif
        computed: {
			...mapState(['hasLogin','userData'])
		},
        methods: {
			//个人信息
			userBash(){
				if(this.userData){
					this.navTo('/pages/user/bash/bash')
					
				}else{
					uni.navigateTo({
					    url: '/pages/user/login/login'
					});
				}
			},
			
	
			/**
			 *  会员卡下拉和回弹
			 *  1.关闭bounce避免ios端下拉冲突
			 *  2.由于touchmove事件的缺陷（以前做小程序就遇到，比如20跳到40，h5反而好很多），下拉的时候会有掉帧的感觉
			 *    transition设置0.1秒延迟，让css来过渡这段空窗期
			 *  3.回弹效果可修改曲线值来调整效果，推荐一个好用的bezier生成工具 http://cubic-bezier.com/
			 */
			coverTouchstart(e){
				if(pageAtTop === false){
					return;
				}
				this.coverTransition = 'transform .1s linear';
				startY = e.touches[0].clientY;
			},
			coverTouchmove(e){
				moveY = e.touches[0].clientY;
				let moveDistance = moveY - startY;
				if(moveDistance < 0){
					this.moving = false;
					return;
				}
				this.moving = true;
				if(moveDistance >= 80 && moveDistance < 100){
					moveDistance = 80;
				}
		
				if(moveDistance > 0 && moveDistance <= 80){
					this.coverTransform = `translateY(${moveDistance}px)`;
				}
			},
			coverTouchend(){
				if(this.moving === false){
					return;
				}
				this.moving = false;
				this.coverTransition = 'transform 0.3s cubic-bezier(.21,1.93,.53,.64)';
				this.coverTransform = 'translateY(0px)';
			},
			gotabbar(index) {
				if(index == 0) {
					uni.navigateTo({
						url: '/pages/tabBar/home/home'
					});
				}
			},
			
        }
    }  
</script>  
<style lang='scss'>
	%flex-center {
	 display:flex;
	 flex-direction: column;
	 justify-content: center;
	 align-items: center;
	}
	%section {
	  display:flex;
	  justify-content: space-around;
	  align-content: center;
	  background: #fff;
	  border-radius: 10upx;
	}

	uni-app {
		background: #f5f5f5;
	}
	.user-section{
		height: 149px;
		padding: 0 30upx 0;
		position:relative;

		.bg{
			position:absolute;
			left: 0;
			top: 0;
			width: 100%;
			height: 100%;
			filter: blur(1px);
			
			
		}
	}
	.user-info-box{
		height: 180upx;
		display:flex;
		align-items:center;
		position:relative;
		z-index: 1;
		.portrait{
			width: 128upx;
			height: 128upx;
			/* border:5upx solid #fff; */
			border-radius: 50%;
			box-shadow: 0 0px 12px #848484;
		}
		.username{
			font-size: $font-lg + 4upx;
			color: $font-color-dark;
			margin-left: 20upx;
			font-weight: bold;
		}
	}

	.vip-card-box{
		
		display:none;
		flex-direction: column;
		color: #f7d680;
		height: 240upx;
		background: linear-gradient(to right, rgba(0,0,0,.7), rgba(0,0,0,.8));
		border-radius: 16upx 16upx 0 0;
		overflow: hidden;
		position: relative;
		padding: 20upx 24upx;
		.card-bg{
			position:absolute;
			top: 20upx;
			right: 0;
			width: 380upx;
			height: 260upx;
		}
		.b-btn{
			position: absolute;
			right: 20upx;
			top: 16upx;
			width: 132upx;
			height: 40upx;
			text-align: center;
			line-height: 40upx;
			font-size: 22upx;
			color: #36343c;
			border-radius: 20px;
			background: linear-gradient(to right, #f9e6af, #ffd465);
			z-index: 1;
		}
		.tit{
			font-size: $font-base+2upx;
			color: #f7d680;
			margin-bottom: 28upx;
			[class*="hxicon-"]{
				color: #f6e5a3;
				margin-right: 16upx;
			}
		}
		.e-b{
			font-size: $font-sm;
			color: #d8cba9;
			margin-top: 10upx;
		}
	}
	.cover-container{
		background: $page-color-base;
		margin-top: -150upx;
		padding: 0 30upx;
		position:relative;
		background: #f5f5f5;
		padding-bottom: 20upx;
		.arc{
			position:absolute;
			left: 0;
			top: -34upx;
			width: 100%;
			height: 36upx;
		}
	}

	.order-section{
		@extend %section;
		padding: 28upx 0;
		margin-top: 20upx;
		.order-item{
			@extend %flex-center;
			width: 120upx;
			height: 120upx;
			border-radius: 10upx;
			font-size: $font-sm;
			color: $font-color-dark;
		}
		[class*="hxicon-"]{
			font-size: 48upx;
			margin-bottom: 18upx;
			color: #ffd21c;
		}
		.icon-shouhoutuikuan{
			font-size:44upx;
		}
	}
	.history-section{
		padding: 30upx 0 0;
		margin-top: 20upx;
		background: #fff;
		border-radius:10upx;
		.sec-header{
			display:flex;
			align-items: center;
			font-size: $font-base;
			color: $font-color-dark;
			line-height: 40upx;
			margin-left: 30upx;
			[class*="hxicon-"]{
				font-size: 44upx;
				color: #5eba8f;
				margin-right: 16upx;
				line-height: 40upx;
			}
		}
		.h-list{
			white-space: nowrap;
			padding: 30upx 30upx 0;
			image{
				display:inline-block;
				width: 160upx;
				height: 160upx;
				margin-right: 20upx;
				border-radius: 10upx;
			}
		}
	}

	.uni-container-tabbar {
		position: fixed;
		bottom: 0;
		background: #ffffff;
		width: 100%;
		height: 60px;
		border-top-left-radius: 20px;
		border-top-right-radius: 20px;
		padding-bottom: 20px;
	}
	.uni-container-tabbar-container {
		display: flex;
		height: 100%;
		flex-direction: initial;
		justify-content: space-around;
		align-items: center;
	}
	.uni-container-tabbar-img img {
		height: 20px;
		width: 20px;
	}
	
</style>