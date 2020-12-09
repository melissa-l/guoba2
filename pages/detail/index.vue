<template>
	<view class="hx-store" @touchstart="touchStart">
		<hx-navbar 
		    :fixed="true"
		    :color="['#ffffff','#888888']"
		    barPlaceholder="hidden"
		    transparent="auto"
			:back="false" 
			:rightSlot="false"
		    :background-color="[245,245,245]"
			:pageScroll.sync="pageScroll">
			<block slot="left">
				<view class="" style="margin-left: 6px; font-size: 22px;" @click="navBack">
					<i class="hxicon-back"></i>
				</view>
				 
			</block>
			<view class="ctn">
				<view class="searchCtn" :style="{'opacity':navSearchBgOpacity}">
					{{storeData.store_name}}
				</view> 
			</view>
		</hx-navbar>
		
		<!-- 只需要绑定购物车位置即可 -->
		<!-- <flyInCart ref="inCart" :cartBasketRect="cartBasketRect"></flyInCart> -->
		<!-- 头部 -->
		<view class="header">
			<image class="header-bg" :src="storeData.banner" mode="" />
			<view :class="showStoreBox ? 'header-bg-gray' : 'header-bg-black'"></view>
			<view class="header-top-Placeholder" ></view>
			<!-- <view class="storeInfo" :style="{height:storeInfoBoxHeight + 'px'}"> -->
			<view class="storeInfo">
				<image class="storeAvatar hx-shadow" :src="storeData.avatar" mode="" />
				<view class="hx-txt-18 hx-color-black hx-txt-weigth ">
					{{storeData.store_name}}
				</view>
				<text class="hx-txt-12 hx-color-black hx-mb-4">起送 ${{storeData.startingPrice}} | 配送 ${{storeData.delivery}}</text>
				<view class="hx-txt-12 hx-color-grey" style="color: rgba(0, 0, 0, 0.54);">{{storeData.notice}}</view>
				<view class="shrink-box">
					<i class="hxicon" :class="showStoreBox ? 'icon-fold' : 'icon-unfold'" @click="showStoreBox = !showStoreBox"></i>
				</view>
			</view>
		</view>
		
		<!-- 主体 -->
		<view class="main" :style="{height: mainHeight}" >
			<!-- <view class="" :style="{display:showZz}" style="position: absolute;top: 0;bottom: 0;left: 0;width: 100%; background: #3F536E;z-index: 999;opacity: 0.5;">
				遮罩
			</view> -->

			<!-- swiper tab -->
			<view class="tabs-box" :style="{'top': 'calc(44px + ' + statusBarHeight + 'px)'}">
				<view class="" style="height: 100%;">
						
					<view class="hx-tabs">
						<view class="hx-tabs-item" v-for="(item,i) in tabs" :key="i" :class="{'hx-tabs-active': swiperCurrent == i}" @click="swiperChange(i)" :style="{transition: transtionTime + 'ms'}">
							<text>{{item.name}}</text>
						</view>
						<view class="hx-tabs-slider-box" :style="{transition: transtionTime + 'ms',left:swiperCurrentSliderLeft + '%'}">
							<view class="hx-tabs-slider"></view>
						</view>
					</view>
				</view>
			</view>

			<swiper 
				id="mainSwiper"
				style="height: 100%;"
				:current="swiperCurrent" 
				:duration="transtionTime"
				@transition="transition"
				@animationfinish="animationfinish">
				<!-- 购物 -->
				<swiper-item class="swiper-item" >
					<view class="scroll-items">
						<view class="category-list">
							<!-- 左侧分类导航 -->
							<scroll-view  :scroll-top="menuScrollTop" :scroll-y="goodsBoxScroll" class="left" >
								<view v-for="item in categoryList" :key="item.id" class="row" :class="{active: item.id == menuCurrentId}" @click="showCategory(item)">
									<view class="text">
										{{item.name}}
									</view>
									<!-- <view class="row-number" v-if="item.number">
										{{item.number}}
									</view> -->
								</view>
							</scroll-view>
							<!-- 右侧子导航 -->
							<scroll-view scroll-with-animation :scroll-y="goodsBoxScroll" class="right"  @scroll="asideScroll" :scroll-top="tabScrollTop" >
								<view class="goodsListBox">
									<view class="category" v-for="item in categoryList" :key="item.id" :id="'goodsBox'+item.id" >
										<view class="s-item">{{item.name}}</view>
										<view class="list"  >
											<view class="box" v-for="(rowData,i) in goodsList" :key="i" v-if="item.id == rowData.type_id">
												<!-- 商品列表 -->
												<view class="m-store-item">
													<view class="m-img" @click="hrefGoodsInfo(rowData.id)">
														<image style="width: 100%;height: 100%;border-radius: 4px;" :src="rowData.img" mode="aspectFit" />
													</view>
													<view class="m-text">
														<view>
															<view class="m-title">{{rowData.name}}</view>
															<view class="m-descripe">{{rowData.descripe}}</view>
														</view>
														<view class="m-text-price-box">
															<view class="m-price-box">
																<view class="symbol">$</view>
																<view class="m-price">{{rowData.price}}</view>
															</view>
															<hx-number-box @change="addGoodsChange" :value="rowData.number" :rowData="rowData" :clickTime="animaTime"></hx-number-box>
														</view>
														
													</view>
												</view>
											</view>
										</view>
									</view>
								</view>
							</scroll-view>
						</view>		
					</view>
				</swiper-item>
				
				<!-- 商家 -->
				<swiper-item class="swiper-item" >
					<view class="scroll-items business-box">
						<view class="hx-txt-14 info-list-title">商家简介</view>
						<view class="info-list">
							<view class="info-list-container">
								<text>{{ storeData.descripe || '新加坡最好的饺子店' }}</text>
							</view>
						</view>
						<view class="hx-txt-14 info-list-title">商家电话</view>
						<view class="info-list">
							<view class="info-list-container">
								<text>{{ storeData.telephone }}</text>
							</view>
						</view>
						<view class="hx-txt-14 info-list-title">商家地址</view>
						<view class="info-list">
							<view class="info-list-container">
								<text>{{ storeData.address }}</text>
							</view>
						</view>
					</view>
				</swiper-item>
			</swiper>
		</view>

		<!-- 购物车 -->
		<view class="foot"  @touchmove.stop.prevent="mpClear" :style="{height: footHeight}" v-if="showFoot">
			<view class="zz" @click="hideShoppingCar"></view>
			<view class="detail-content-footer">
				<view class="detail-content-footer-shopping">
					<view @click="showShoppingCar()" class="detail-content-footer-shopping-menu" :class="{ 'detail-content-footer-shopping-menu-have': goodsTotalPrice > 0 }">
						<img :src="true ? '/static/shopping2.png' : '/static/shopping.png'" class="detail-content-footer-shopping-logo" />
						<text class="detail-content-footer-shopping-price">$ {{goodsTotalPrice}}</text>
						<text v-if="goodsTotalPrice < 1"> | 还差$ {{(100 - goodsTotalNumber) > 0 ? (100 - goodsTotalNumber) : 0 }}起送</text>
						<view v-if="goodsTotalPrice > 0" class="detail-content-footer-shopping-value">{{goodsTotalNumber}}</view>
					</view>
					<view class="detail-content-footer-shopping-go" :style="{ background: goodsTotalPrice > 0 ? '#EE5B2D' : ''}" @click="goodsTotalPrice > 0 ? goPintuan() : undefined">发起拼团</view>
				</view>
			</view>
			<view class="cart-box" :style="{display: showCar ? 'flex' : 'none'}">
				<view class="box-container operating-box">
					<view class="operating-box_right">
						已选
					</view>
					<view class="operating-box_left clear" @click="clearShoppingCart">
						<i class="hxicon-delete"></i>
						<text>清空购物车</text>
					</view>
				</view>
				<view class=" goods-box">
					<view class="" style="flex: 1;">
						<scroll-view scroll-y="true" class="goods-list-scroll" :scroll-top="carGoodsScrollTop">
							<view class="goods-list">
								<view class="box" v-for="(rowData,i) in shoppCart" :key="rowData.id" v-if="rowData.number>0">
									
									<view class="m-store-item">
										<view class="m-text">
											<view class="m-title">
												{{rowData.name}}
											</view>
										
										</view>
										<view class="m-price-distance">
											<view class="m-price-box" >
												<view class="symbol">$</view>
												<view class="m-price">{{rowData.price}}</view>
												<view class="m-old-price" v-if="rowData.oldprice">
													<text>￥{{rowData.oldprice}}</text>
												</view>
											</view>
											<view class="m-distance" > 
												<view :class="'addEle2_' + rowData.id" class="jumpPosition">
												</view>
												<hx-number-box @change="addGoodsChange" :value="rowData.number" :rowData="rowData" :clickTime="animaTime" @addChange="touchOnAddGoods('.addEle2_' + rowData.id,rowData)"></hx-number-box>
											</view>
										</view>
									</view>
								</view>
							</view>
						</scroll-view>
					</view>
					
				</view>
			</view>
			
		</view>
		<uni-popup ref="popup" type="dialog">
			<uni-popup-dialog type="warning" mode="base" content="确认清空购物车所有项目吗?" :duration="2000" :before-close="true" @close="closeDialog" @confirm="confirmDialog"></uni-popup-dialog>
		</uni-popup>
	</view>
</template> 

<script>
	import uniIcons from '@/components/uni-icons/uni-icons.vue';
	import hxNavbar from '@/components/hx-navbar/hx-navbar.vue';
	import jumpBall from '@/components/hx-jump-ball/hx-jump-ball.vue';
	import hxNumberBox from "@/components/uni-number-box/uni-number-box.vue";
	import hxComment from "@/components/hx-comment/hx-comment.vue";
	import uniPopup from '@/components/uni-popup/uni-popup.vue';
	import uniPopupDialog from '@/components/uni-popup/uni-popup-dialog.vue';

	//  加入购物车动画组件
	import flyInCart from '@/components/flyInCart.vue'
	//引入测试数据
	import testData from "@/common/testdata.js";
	
	var statusBarHeight = uni.getSystemInfoSync().statusBarHeight;
	
	export default {
		components: {
			jumpBall,
			hxNavbar,
			uniIcons,
			hxNumberBox,
			hxComment,
			flyInCart,
			uniPopup,
			uniPopupDialog
		},
		data() {
			return {
				pageScroll:{},
				//商家信息
				storeData: [],
				//商品列表
				goodsList: [],
				//商品分类信息列表
				categoryList: [],
				//评论列表
				commentList: [],
				menuHave: false,
				
				navSearchWidth: 0,
				navSearchBgOpacity: 0,
				navSearchColor: '#ffffff',
				navHeadHeight: 44,
				//默认禁止商品栏滚动
				goodsBoxScroll: false,
				statusBarHeight,
				//动画时间
				animaTime: 300,
				//商家盒子高度
				storeInfoBoxHeight: 100,
				//展开门店信息容器
				showStoreBox: false,
				
				num:1,
				element: [],				
				cartAnimation: {},
				cartAnimationData: {},
				
				contentPath: '/static/rectangle.png',

				cart: [],
				//tabs
				tabs: [
					{name:'菜单'},
					{name:'商家信息'},
				],
				swiperCurrent: 0,
				dx: 0,
				swiperCurrentSliderLeft: 15,
				transtionTime:100,
				
				//所有购物车
				shoppingCartAll:[],
				//显示购物车
				showFoot:true,
				allPrice: 0,
				allValue: 0,
				//配送费
				shippingDees: 0,
				//配送起步价
				startingPrice:0,
				
				//购物车商品价格合计
				goodsTotalPrice: 0,
				//购物车商品数量合计
				goodsTotalNumber: 0,
				//当前门店购物车
				shoppCart: [],
				//foot 高度
				footHeight: '0',
				//显示购物车
				showCar: false,
				//购物车中商品滚动条位置
				carGoodsScrollTop: 0,
				//购物车缓存 Storage 名称
				shoppingCartStorageName: 'shopping_cart',
				
				sizeCalcState: false,
				tabScrollTop: 0,
				
				mainHeight: 0,
				menuCurrentId:1,
				
				
				//购物车显示折扣
				showDiscount:true,
				//手指触摸滑动
				touchData:{},
				//页面滚动条距离顶部的距离
				pageScrollTop: 0,
				
				menuScrollTop: 0,
				
				//多规格当前产品
				currentGoodsData: {},
				//显示规格容器
				showFormBox: false,
				//显示规格动画
				formAnimationData:{},
				
				//购物车位置数据
				cartBasketRect:{},
			
				navStatus: true,
				isBack:true,
			}
		},
		async onLoad(option) {
			const that = this
			uni.showLoading({
			    title: '加载中'
			});
			
			
			//模拟请求数据
			setTimeout(()=>{
				//商家信息
				that.storeData = testData.storeData,
				//商品列表
				that.goodsList = testData.goodsData;
				//商品分类信息列表
				that.categoryList = testData.categoryData;
				//评论列表
				that.commentList = testData.commentData,
				
				setTimeout(()=>{
					this.init();
					uni.hideLoading();
				},200)
			},500)

			// #ifdef MP-WEIXIN
			uni.login({
				provider: 'weixin',
				success: function (loginRes) {
					console.log(loginRes);
					uni.request({
					 	url: "https://guoba.online/guoba/user/" + loginRes.code
					})
					.then(data => {//data为一个数组，数组第一项为错误信息，第二项为返回数据
						var [error, res]  = data;
						console.log(res.data);
					})

					// 获取用户信息
					uni.getUserInfo({
						provider: 'weixin',
						success: function (infoRes) {
							console.log('用户昵称为：' + infoRes.userInfo.nickName);
						}
					});
				}
			});
			// #endif
		},
		onReady() {
			const that = this
			let sysInfo = uni.getSystemInfoSync();
			//屏幕高度 - 头部导航高度 - 状态栏高度 - 分页高度
			this.mainHeight = sysInfo.screenHeight - 43 - statusBarHeight - 40  + 'px';
			
			let q = uni.createSelectorQuery()
			setTimeout(function(){
				q.select('.cart').boundingClientRect(data => {
				 that.cartBasketRect = data
				}).exec();
			},100)
			that.calcSize();
		},
		onShow() {
			this.init();
		},
		mounted() {
			let that = this
			
		},
		watch:{
			showStoreBox(val,oldVal){
				if(val == true){
					console.log('del')
				}else{
					this.hiddenStoreBoxFunc();
				}
			}
		},
		
		onPageScroll(e) {
			let that = this
			let top = e.scrollTop
			that.pageScrollTop = e.scrollTop
			that.pageScroll = e
			
			if(top < 120){
				if(that.navSearchWidth >= 3){
					if(top<3){
						that.navSearchWidth = 3
						that.navSearchBgOpacity = 0
						that.navSearchColor = '#ffffff'
					}else{
						let n = top * (120/100)
						if(n > 100){
							n = 100
						}
						that.navSearchWidth = n
						//that.navSearchBgOpacity =1
						that.navSearchBgOpacity = top * (1/100)
						that.navSearchColor = '#888888'
					}
				}
			}else{
				that.navSearchWidth = 100
				that.navSearchBgOpacity = 1
				that.navSearchColor = '#888888'
			}
			
			let view = uni.createSelectorQuery().select(".main");
			view.fields({
				rect: true
			}, data => {
				if(data != null){
					if(data.top <= statusBarHeight+44){
						that.goodsBoxScroll = true
					}else{
						that.goodsBoxScroll = false
					}
				}
				
			}).exec();
		},
		methods: {
			
			init(){

				let that = this;
				//假设这是从后台获取的商品数据
				let goods = this.goodsList;
				//商品初始化
				// for(let i in goods){
				// 	goods[i].purchases = 3
				// }
				that.shoppCart = []

				let carts = uni.getStorageSync(that.shoppingCartStorageName) || [];
				console.log(carts);
				//根据缓存数据 获取购物车中属于本商店的商品
				for(let i in carts){
					if(carts[i].store_id == that.storeData.store_id){
						that.shoppCart = carts[i].shopping_cart  ? carts[i].shopping_cart : [];
						break;
					}
				}

				that.goodsTotalPrice = 0;
				that.goodsTotalNumber = 0;
				for(let i in that.shoppCart){
					for(let j in goods){
						if(goods[j].id == that.shoppCart[i].id){
							goods[j].number = that.shoppCart[i].number
						}
					}
					//计算商品总价
					that.goodsTotalPrice += that.shoppCart[i].total
					//商品总数量
					that.goodsTotalNumber += Number(that.shoppCart[i].number)
				}
				//初始化商品列表
				that.goodsList = goods;
				//初始化起步价和配送费
				that.starting_price = that.storeData.starting_price;
				that.shipping_dees = that.storeData.shipping_dees;
				
				that.setLabelNumber();
			},
			goPintuan: function() {
				uni.navigateTo({
					url: '/pages/confirm/index?cart=' + encodeURIComponent(JSON.stringify(this.cart)) + 
					'&allPrice=' + this.allPrice});
			},
			navBack(){
				if(getCurrentPages().length>1){
					uni.navigateBack();
				}else{
					// #ifdef H5
					history.back()
					// #endif
					// #ifndef H5
					uni.reLaunch({
						url: '/pages/index/index'
					});
					// #endif
				}
			},
			//-----------------------------------------------------------------------------------
			//显示 规格
			showForm(goods){
				var that = this;
				let goodsCart = that.getStoreCart();
			
				if(goodsCart){
					let currentGoods = null
					for(let i in goodsCart){
						if(goodsCart[i].id == goods.id){
							currentGoods = goodsCart[i]
							break
						}
					}
					if(currentGoods){
						let selectStatus = false
						for (let i in goods.form_child){
							if(goods.form_child[i].id == currentGoods.current_form.id){
								if(!selectStatus){
									goods.form_child[i].select = true
									goods.number = currentGoods.number
									selectStatus = true
								}else{
									goods.form_child[i].select = false
								}
							}else{
								goods.form_child[i].select = false
							}
						}
					}
					
				}
				that.currentGoodsData = goods
			},
			closeDialog(done){
				done()
			},
			confirmDialog(done){
				this.allValue = 0;
				this.allPrice = 0;
				const cart = [...this.cart];
				cart.map(item => {
					item.value = 0;
				})
				this.cart = [];
				this.menuHave = false;
				this.$refs.drawer.close();
				this.$forceUpdate();
				done()
			},
			openCart() {
				if (this.allValue > 0) {
					this.$refs.drawer.open();
				}
			},
			//获取该商品在购物车的数量
			getCartGoodsNum(goods){
				let cart = this.getStoreCart()
				let n = 0
				if(cart){
					for(let i in cart){
						if(cart[i].id == goods.id){
							n += cart[i].number
						}
					}
				}
				return n
			},
			//-----------------------------------------------------------------------------------
			//获取门店购物车
			getStoreCart(){
				const that = this
				return that.shoppCart
			},
			
			//---------------------------------------------------------------------------------
	
			
			swiperChange(index) {
				this.swiperCurrent = index;
				this.swiperCurrentSliderLeft= index ? 65 : 15;
			},
			transition({ detail: { dx } }) {
				// this.$refs.tabs.setDx(dx);
			},
			animationfinish({detail: { current }}) {
				/* this.$refs.tabs.setFinishCurrent(current); */
				this.swiperCurrent = current;
				this.current = current;
				this.swiperChange(current);
				this.showFoot = current == 0 && this.showStoreBox != true ? true : false;
				
			},
			
			//一级分类点击
			showCategory(item){
				
				const that = this
				that.isBoxScroll = true;
				that.menuCurrentId = item.id;
				setTimeout(()=>{
					let index = that.categoryList.findIndex(sitem=>sitem.id === item.id);
					that.tabScrollTop = that.categoryList[index].top;
					that.isBoxScroll = false
				},100)
				
			},
			del(del) {
				console.log(del);
				this.allValue = this.allValue - 1;
				this.allPrice = this.allPrice - del.price;

				if (this.allValue === 0) {
					this.$refs.drawer.close();
					this.menuHave = false;
				}
				const cart = [...this.cart];
				cart.map((item, index) => {
					if (item.id === del.id) {
						del.value--;
						if (del.value === 0) {
							cart.splice(index, 1);
						}
					}
				})
				this.cart = cart;
				this.$forceUpdate();
			},
			//右侧栏滚动
			asideScroll(e){
				const that = this;
				const scrollTop = Math.round(e.detail.scrollTop);
				that.calcSize()
				const tabs = that.categoryList.filter(item=>item.top <= scrollTop).reverse();
				
				if(tabs.length > 0){
					that.menuCurrentId = tabs[0].id;
				}
				const menuNum = that.categoryList.length
				const cNum = tabs.length
				// 定位在第4个分类，当分类滑动到第4格时将不再变换位置。
				const n = 4
				if(cNum>n){
					that.menuScrollTop = (cNum - n) * 45
				}else{
					that.menuScrollTop = 0
				}
			},
			//计算右侧栏每个tab的高度等信息
			calcSize(event){
				let h = 0;
				// if(this.sizeCalcState){
				// 	return false
				// }
				this.categoryList.forEach(item=>{
					let view = uni.createSelectorQuery().select("#goodsBox" + item.id);
					view.fields({
						size: true
					}, data => {
						if(data != null){
							item.top = h;
							h += Math.round(data.height);
							item.bottom = h;
						}
						
					}).exec();
				})
				this.sizeCalcState = true;
			},
			
			//新增商品计算价格
			addGoodsChange(num, rowData){
				var number = num;
				var that = this;
				
				let shoppCart = [];
				let totalPrice = 0;
				let totalNumber = 0;
				let existedGoods = false;
				//门店第一次添加商品
				let isFirstAddGoods = true;
				//是否为有规格的商品
				let isFormGoods = false
				if(rowData.current_form){
					isFormGoods = true
				}
				let deleteGoods = null
				let carts = uni.getStorageSync(that.shoppingCartStorageName) || [];
			
				if(carts.length != 0){
					isFirstAddGoods = false
					//根据缓存数据 获取购物车中属于本商店的商品
					for(let i in carts){
						if(carts[i].store_id == that.storeData.store_id){
							shoppCart = carts[i].shopping_cart ? carts[i].shopping_cart : [];
							break;
						}
					}
					//检查该商品是否为第一次添加，
					for(var i in shoppCart){
						if(shoppCart[i].id == rowData.id){
							if(isFormGoods){
								//相同商品比较规格是否也相同
								if(shoppCart[i].current_form.id == rowData.current_form.id){
									existedGoods = true;
								}
							}else{
								existedGoods = true;
							}
							if(existedGoods){
								//在购物车中移除该商品
								if(number <= 0){
									deleteGoods = shoppCart[i];
									break;
								}
								//非第一次添加，直接修改商品数量，并计算出单品合计
								if(isFormGoods){
									shoppCart[i].price = rowData.price
									shoppCart[i].total = number *  rowData.price
								}else{
									shoppCart[i].price = rowData.price
									shoppCart[i].total = number *  rowData.price
								}
								shoppCart[i].number = rowData.number = number
								break;
							}
						}
					}
				}
				//在购物车中移除该商品
				if(deleteGoods != null){
					if(carts){
						//根据缓存数据 获取购物车中属于本商店的商品
						for(let i in carts){
							if(carts[i].store_id == that.storeData.store_id){
								var index = shoppCart.indexOf(deleteGoods);
							
								if (index > -1) { 
									shoppCart.splice(index, 1); 
								} 
								carts[i].shopping_cart = shoppCart
								uni.setStorageSync(that.shoppingCartStorageName,carts);
								break;
							}
						}
						setTimeout(()=>{
							this.init();
						},100)
					}else{
						that.storeData.shopping_cart = []
						uni.setStorageSync(that.shoppingCartStorageName,that.storeData);
					}
					
					return 
				}
					
				//第一次添加
				if(!existedGoods){
					if(rowData.form){
						rowData.price = (rowData.price)
						rowData.total = Number(number * rowData.price)
					}else{
						rowData.total = Number(number * rowData.price)
					}
					rowData.number = number;
					shoppCart.push(rowData);
				}
				
				//计算总商品数量和总价
				for(var i in shoppCart){
					//总价
					totalPrice += shoppCart[i].total
					totalNumber += Number(shoppCart[i].number)
				}
				//更改商品列表中的已购买数量
				for(let i in that.goodsList){
					if(that.goodsList[i].id == rowData.id){
						that.goodsList[i] = rowData
						break;
					}
				}
				
				that.goodsTotalPrice = totalPrice
				that.goodsTotalNumber = totalNumber
				
				that.shoppCart = shoppCart; 
				that.storeData.shopping_cart = shoppCart;
				
				that.setLabelNumber();
				if(isFirstAddGoods){
					carts.push(that.storeData)
				}
				if(that.goodsTotalNumber == 0){
					that.hideShoppingCar();
				}
				console.log('change', carts)
				//购物车商品数据缓存至本地
				uni.setStorageSync(that.shoppingCartStorageName,carts);
			},
			
			//计算每类商品购买的总数
			setLabelNumber(){
				let that = this;
				//计算每一类购买商品的总数量
				for(let j in that.categoryList){
					let n = 0;
					for(var i in that.shoppCart){
						if(that.shoppCart[i].type_id ==  that.categoryList[j].id){
							n += that.shoppCart[i].number;
						}
					}
					that.categoryList[j].number = n;
				}
			},
			
			//去结算
			jiesuan(){
				
				this.navTo("/pages/order/preview?sid=" + this.storeData.store_id)
				
			},
			navTo(url){
				let that = this;
				if(that.navStatus){
					that.navStatus = false
					uni.navigateTo({
						url: url,
						complete:function(){
							that.navStatus = true
						}
					})
				}
			},
			//联系商家
			contact(){
				uni.showModal({
					title:"",
					content:"联系商家"
				})
			},
			hiddenStoreBoxFunc(){
				this.storeInfoBoxHeight = 100;
				this.showStoreBox = false;
				if(this.swiperCurrent == 0){
					this.$set(this.$data,'showFoot',true);
				}
			},
			
			mpClear(e) {
				// TODO nvue 取消冒泡
				e.stopPropagation()
			},
			
			//显示购物车
			showShoppingCar(){
				if(this.goodsTotalNumber == 0){
					return;
				}
				this.footHeight = '100%';
				this.showCar = true;
				this.carGoodsScrollTop = 0;
			},
			//隐藏购物车
			hideShoppingCar(){
				this.footHeight = '0';
				this.showCar = false;
			},
			//清空该门店的购物车
			clearShoppingCart(){
				let that = this;
				that.shoppCart = [];
				that.storeData.shopping_cart = [];
				for(let i in that.shoppingCartAll){
					if(that.shoppingCartAll[i].store_id == that.storeData.store_id){
						that.shoppingCartAll[i] = that.storeData
					}
				}
				uni.setStorageSync(that.shoppingCartStorageName,that.shoppingCartAll);
				
				for(let i in that.goodsList){
					that.goodsList[i].number = 0;
				}
				for(let i in that.categoryList){
					that.categoryList[i].number = 0;
				}
				//购物车商品价格合计
				that.goodsTotalPrice = 0;
				//购物车商品数量合计
				that.goodsTotalNumber = 0;
				that.hideShoppingCar();
			},
			hrefGoodsInfo(id){
				
				this.navTo('/pages/product/product?id=' + id)
			},
			
			touchStart(e){
			                    
			    this.touchData.clientX=e.changedTouches[0].clientX;
			                 
			    this.touchData.clientY=e.changedTouches[0].clientY;
			},
		}
	}
</script>

<style lang="scss">
	//主题颜色
	$hx-theme-color: #FFC107;
	$hx-theme-color-light: #FFEB3B;

	.detail-content-footer {
		position: fixed;
		bottom: 0;
		width: 100%;
		z-index: 1000;
	}
	.detail-content-footer-shopping {
		display: flex;
		flex-direction: initial;
		padding: 12px 9px 32px 15px;
		background: #ffffff;
	}
	.detail-content-footer-shopping-menu {
		width: 65.8%;
		position: relative;
		display: flex;
		flex-direction: inherit;
		background: #e8e8e8;
		align-items: center;
		padding: 11px 0 11px 14px;
		border-top-left-radius: 5px;
		border-bottom-left-radius: 5px;
		color: rgba(0, 0, 0, 0.26);
	}
	.detail-content-footer-shopping-menu-have {
		background: rgba(0, 0, 0, 0.87);
		color: #ffffff;
	}
	.detail-content-footer-shopping-logo {
		width: 25px;
		height: 26px;
		margin-right: 10px;
	}
	.detail-content-footer-shopping-price {
		font-size: 16px;
		font-weight: 500;
		margin-right: 8px;
	}
	.detail-content-footer-shopping-go {
		background: #cccccc;
		color: #ffffff;
		width: 33%;
		border-top-right-radius: 5px;
		border-bottom-right-radius: 5px;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 16px;
		font-weight: 500;
	}
	.detail-content-footer-shopping-value {
		position: absolute;
		width: 15px;
		height: 15px;
		left: 33px;
		top: 9px;
		background: rgba(238, 77, 45, 1);
		border-radius: 50%;
		font-size: 10px;
		text-align: center;
		line-height: 14px;
		border: 1px solid #ffffff;
	}

	.add{
		position: fixed;
		right: 60upx;
		top: 300upx;
		z-index: 999;
	}
	.ctn{
		
		/* border: 1px solid #e3e3e3; */
		width: 100%;
		display: flex;
		justify-content: center;
		overflow: hidden;
		align-items: center;
		.searchCtn{
			display: flex;
			border-radius: 80upx;
			padding: 8upx 12upx;
			padding-right: 40px;
			line-height: 44upx;
			overflow: hidden;
			align-items: center;
			min-width: 22px;
			color: #000000;
		}
		.leftBox{
			display: flex;
			width: 53px;
			justify-content: space-between;
			flex: none;
			margin: 0 8px;
		}
		.jrNull{
			/* #ifdef MP */
			width: 95px;
			flex: none;
			/* #endif */
		}
	}
	
	
	page{
		background: #ffffff;
	}
	.hx-bb{
		border-bottom: 1px solid $uni-border-color;
	}
	.hx-txt-10{
		font-size: 10px;
	}
	.hx-txt-12{
		font-size: 12px;
	}
	.hx-txt-14{
		font-size: 14px;
	}
	.hx-txt-16{
		font-size: 16px;
	}
	.hx-txt-18{
		font-size: 18px;
	}
	.hx-txt-22{
		font-size: 22px;
	}
	.hx-color-gray{
		color: #bbbbbb;
	}
	.hx-color-white{
		color: #FFFFFF;
	}
	.hx-color-black{
		color: #333333;
	}
	.hx-txt-weigth{
		font-weight: bold;
	}
	.hx-mb-10{
		margin-bottom: 10px;
	}
	.hx-mt-15{
		margin-top: 15px;
	}
	.hx-shadow{
		box-shadow: 0px 6upx 16upx rgba(173, 173, 173, 0.2);
	}
	.miniBtn{
		position: relative;
		padding: 0 12px;
		border-radius: 20px;
		height: 24px;
		line-height: 24px;
		text-align: center;
		background: linear-gradient(100deg, #FFEB3B, #FFC107);
		font-size: 10px;
		color: #333;
		.num{
			position: absolute;
			right: 0;
			top: -10px;
			width: 18px;
			height: 18px;
			line-height: 18px;
			font-size: 10px;
			color: #fff;
			background-color: #ff5722;
			text-align: center;
			border-radius: 50%;
			
		}
	}
	.hx-store{
		.header{
			position: relative;
			// min-height: 230px;
			&-bg{
				position: absolute;
				left: 0;
				top: 0;
				z-index: 2;
				width: 100%;
				height: 142px;
			}
			&-bg-black{
				position: absolute;
				left: 0;
				top: 142px;
				bottom: 0;
				z-index: 1;
				background-color: #ffffff;
				width: 100%;
				transition: background-color 0.2s;
			}
			&-bg-gray{
				position: absolute;
				left: 0;
				top: 142px;
				bottom: -16px;
				z-index: 1;
				background-color: #afafaf;
				width: 100%;
				transition: background-color 0.2s;
			}
			&-top-Placeholder{
				height: 105px;
			}
			.storeInfo{
				position: relative;
				z-index: 2;
				background: #ffffff;
				// height: 100px;
				padding: 12px;
				margin-bottom: 4px;
				transition: all 0.2s;
				.shrink-box{
					position: absolute;
					bottom: 0;
					left: 0;
					right: 0;
					text-align: center;
					font-size: 20px;
					color: #a2a8ab;
				}
				.storeAvatar{
					position: absolute;
					width: 60px;
					height: 60px;
					right: 16px;
					top: -25px;
					background: #ffffff;
					border-radius: 4px;
					
				}
			}
		}

		.container{
			margin: 0 32upx;
		}
		.tabs-box{
			width: 100%;
			position: sticky;
			top: calc(44px + var(--status-bar-height));
			z-index: 10;
			background-color: white;
			border-bottom: 1px solid #efefef;
			border-top: 1px solid #efefef;
			height: 40px;
			
			.hx-tabs{
				position: relative;
				display: flex;
				justify-content: space-around;
				height:100%;
				&-item{
					display: flex;
					flex-direction: row;
					justify-content: center;
					align-items: center;
					width: 70px;
					color:#666666;
					text{
						font-size: 14px;
					}
					
				}
				&-active{
					color:#333333;
				}
				&-slider-box{
					position: absolute;
					display: flex;
					justify-content: center;
					bottom: 0;
					width: 70px;
				}
				&-slider{
					display: flex;
					background: #f6d200;
					width: 50px;
					height: 2px;
				}
			}
		}
		.main{
			position: relative;
			background-color: #ffffff;
			
			#mainSwiper{
				background-color: #f5f5f5;
				position: sticky;
				top: calc(40px + 44px + var(--status-bar-height));
				.scroll-items{
					// 商品列表样式
					.category-list{
						width: 100%;
						background-color: #fff;
						display: flex;
						padding-bottom: 50px;
						
						.left,.right{
							background: #fff;
							position: absolute;
							top:0;
							bottom: 0upx;
						}
						.left{
							width: 22%;
							left: 0upx;
							background-color: #f6f3f3;
							
							.row{
								width: 100%;
								height: 42px;
								display: flex;
								align-items: center;
								overflow: hidden;
								position: relative;
								.text{
									width: 100%;
									font-size: 12px;
									color:#999999;
									overflow: hidden;
									text-overflow: ellipsis;
									display: -webkit-box;
									-webkit-box-orient: vertical;
									-webkit-line-clamp: 2;
									text-align: center;
									padding: 0 16px;
								}
								&-number{
									position: absolute;
									width: 18px;
									height: 18px;
									right: 4px;
									top: 4px;
									background: #ff5722;
									border-radius: 50%;
									line-height: 18px;
									text-align: center;
									font-size: 10px;
									color: #ffffff;
								}
								&.active{
									background-color: #fff;
									.text{
										color: rgba(0, 0, 0, 1);
									}
								}
							}
							.row:last-child{
								margin-bottom: 200upx;
							}
						}
						.right{
						   width: 78%;
							left: 22%;
							.goodsListBox{
								padding-bottom: 100px;
								.category{
					// 				width: 94%;
									padding: 0 15px 10px 15px;
									.s-item{
										height: 40px;
										line-height: 45px;
										font-size: 12px;
										background: #ffffff;
										color: #555555;
										position: sticky;
										top: 0;
										z-index: 18;
									}
									.list:last-child{
										margin-bottom: 0;
									}
									.list{
										margin-bottom: 20px;
										width: 100%;
										display: flex;
										flex-wrap: wrap;
										.box:first-child{
											.m-store-item{
												margin-top: 0;
											}
										}
										.box{
											width: 100%;
											
											.m-store-item{
												display: flex;
												flex-direction: row;
												width: 100%;
												justify-content: space-between;
												align-items: flex-end;
												margin-top: 15px;
												.m-img{
													flex: 0 0 88px;
													height: 88px;
													background: #eee;
													border-radius: 4px;
												}
												.m-text{
													flex: 1;
													position: relative;
													height: 88px;
													padding: 0 6px;
													display: flex;
													align-content: space-between;
													justify-content: space-between;
													flex-direction: column;
													.m-title{
														font-size: 14px;
														color:#555555;
														height: 21px;
														font-weight: bold;
													}
													.m-descripe{
														font-size: 12px;
														color:#999999;
													}
													.m-text-price-box {
														display: flex;
														justify-content: space-between;
													}
													.m-price-box{
														height: 24px;
														display: flex;
														flex-direction: row;
														align-items: flex-end;
														.symbol{
															font-size: 16px;
														}
														.m-price{
															position: relative;
															font-size: 16px;
														}
														.m-old-price{
															margin-left: 3px;
															display: flex;
															flex-direction: row;
															font-size: 10px;
															color:#999999;
															margin-top: 5upx;
															text-decoration: line-through;
															font-weight: normal;
														}
													}
													.m-distance{
														position: absolute;
														right: 0;
														bottom: -4px;
														z-index: 16;
														color:#b2b2b2;
														font-size: 20upx;
														text-align: right;
														.jumpPosition{
															position: absolute; 
															bottom: 23px;
															right: 0;
															z-index: 2;
															width: 28px;
															height: 28px;
														}
													}
													
												}
												
											}
										}
									}
								}
							}
						}
					}
				}
				.evaluate-box{
					
				}
				.business-box{
					.info-list-title {
						color: rgba(0, 0, 0, 0.54);
						margin-top: 12px;
						margin-left: 12px;
					}
					.info-list{
						background: #ffffff;
						padding: 0 15px;
						margin-top: 8px;
						&-container{
							line-height: 46px;
							height: 48px;
							display: flex;
							flex-direction: row;
							color: rgba(0, 0, 0, 0.87);
							text{
								font-size: 14px;
							}
						}
						
					}
				}
			}
			
		}
		.foot{
			position: fixed;
			display: flex;
			z-index: 999;
			
			justify-content:center;
			align-items : center; 
			bottom: 0;
			height: 100%;
			width: 100%;
			
			/*width: calc(100% - 32px);*/
			.btn-box{
				position:absolute;
				display: flex;
				bottom: 15px;
				justify-content:center;
				align-items : center; 
				margin:0;
				height: 50px;
				width: calc(100% - 32px);
				z-index: 9;
				&-left{
					background: #222222;
					border-top-left-radius:50px;
					border-top-right-radius:9px;
					border-bottom-left-radius:50px;
					border-bottom-right-radius:9px;
					padding-left: 6upx;
					display: flex;
					flex-direction:column;
					justify-content:center;
					align-self: center;
					width: 70px;
					height: 100%;
					color: #f6d200;
					text-align: center;
					.imgBox{
						display: flex;
						text-align: center;
						justify-content:center;
						image{
							width: 20px;
							height: 20px;
						}
					}
					text{
						font-size: 20upx;
					}
				}
				&-line{
					background: #ffffff;
					width: 2px;
					height: 100%;
				}
				&-center{
					height: 100%;
					flex: auto;
					display: flex;
					justify-content:flex-start;
					align-self: center;
					align-items: center;
					background: #222222;
					border-top-left-radius:8upx;
					border-bottom-left-radius:8upx;
					padding-left: 10upx;
					.cart{
						position: relative;
						width: 36px;
						height: 36px;
						image{
							width: 100%;
							height: 100%;
						}
						.tag{
							position: absolute;
							right: 12upx;
							top: 16upx;
							height: 18px;
							width: 18px;
							background-color: #ff4000;
							color: #ffffff;
							border-radius: 50%;
							z-index: 1;
							font-size: 10px;
							text-align: center;
							line-height: 18px;
						}
					}
					.priceBox{
						flex: auto;
					}
					
				}
				&-right{
					width: 70px;
					height: 100%;
					position: relative;
					display: flex;
					justify-content:flex-start;
					align-self: center;
					align-items: center;
					
					
					.pscj{
						width: 100%;
						height: 100%;
						border-top-right-radius:100upx;
						border-bottom-right-radius:100upx;
						background: #222222;
						text-align: center;
						display: flex;
						justify-content:center;
						align-self: center;
						align-items: center;  
					}
					.jiesuan{
						width: 100%;
						height: 100%;
						font-size: 28upx;
						border-top-right-radius:100upx;
						border-bottom-right-radius:100upx;
						text-align: center;
						display: flex;
						justify-content:center;
						align-self: center;
						align-items: center;    
						background: linear-gradient(45deg, $hx-theme-color-light, $hx-theme-color); 
						font-weight: bold;
						color: #222222;
					}
				}
			}
			.zz{
				position: absolute;
				top: 0;
				left: 0;
				right: 0;
				bottom: 0;
				background-color: rgba(0,0,0,.7);
				z-index: 1;
			}
			.cart-box{
				display: flex;
				justify-content: flex-start;
				flex-flow: column;
				background: #ffffff;
				position: absolute;
				bottom: 0; 
				z-index: 300;
				max-height: 60%;
				padding-bottom: 95px;
				overflow: hidden;
				width: 100%;
				.rebate-box{
					height: 30px;
					background: #FFC107;
					color: #FF9900;
					text-align: center;
					line-height: 30px;
					font-size: 14px;
				}
				.box-container{
					box-sizing: border-box;
					padding:0 16px;
				}
				.operating-box{
					font-size: 12px;
					line-height: 35px;
					height: 35px;
					border-bottom: 1px solid #f6f6f6;
					display: flex;
					flex-direction: row;
					background: #F5F5F5;
					color: rgba(0, 0, 0, 0.54);
					&_right{
						flex: 1;
					}
					&_left{
						display: flex;
						flex-direction: row;
					}
					
				}
				.goods-box{
					height: 100%;
				    overflow: hidden;
					flex: 1;
					display: flex;
					.goods-list-scroll{
						height: 100%;
						
						.goods-list{
							
							width: 100%;
							display: flex;
							flex-wrap: wrap;
							
							.box{
								width: 100%;
								border-bottom: 1px solid #f6f6f6;
								box-sizing: border-box;
								padding: 0 12px;
								.m-store-item{
									display: flex;
									flex-direction: row;
									width: 100%;
									justify-content: space-between;
									align-items: flex-end;
									position: relative;
									.m-text{
										position: relative;
										height: 48px;
										padding: 0 6px;
										display: flex;
										justify-content: center;
										align-items: center;
										.m-title{
											font-size: 16px;
											color:#555555;
											height: 21px;
											font-weight: 500;
										}
									
									}
									.m-price-distance {
										display: flex;
										height: 48px;
										align-items: center;
									}
									.m-price-box {
										height: 24px;
										font-weight: bold;
										display: flex;
										flex-direction: row;
										align-items: flex-end;
										padding-right: 20px;
										color:#ff582b;
										.m-price{
											position: relative;
											top: 2px;
											font-size: 18px;
										}
										.m-old-price{
											margin-left: 3px;
											display: flex;
											flex-direction: row;
											font-size: 10px;
											color:#999999;
											margin-top: 5upx;
											text-decoration: line-through;
											font-weight: normal;
										}
									}
									.m-distance {
										color:#b2b2b2;
										font-size: 20upx;
										.jumpPosition{
											position: absolute; 
											bottom: 23px;
											right: 0;
											z-index: 2;
											width: 28px;
											height: 28px;
										}
									}
								}
							}
						}
					}
				}
			}
		}
	}
	.form-main{
		
		display: flex;
		flex-direction: column;
		justify-content: left;
		padding: 15px;
		
		.form-main_ctn{
			display: flex;
			flex-direction: column;
			justify-content: left;
			padding: 23px 22px 22px 22px;
			background-color: #fff;
			border-radius: 8px;
			.godos_tit{
				margin-top: 4px;
				margin-bottom: 6px;
				font-size: 18px;
				font-weight: bold;
				color: #333; 
			}
			.gg_tit{
				margin-top: 8px;
				font-size: 14px;
				font-weight: bold;
				color: #555;
			}
			.gg_box{
				margin-top: 8px;
				display: flex;
				flex-direction: row;
				flex-wrap: wrap;
				font-size: 12;
				color: #333;
				.item{
					margin-right: 14px;
					margin-bottom: 14px;
					border: 1px solid #f1f1f1;
					border-radius: 4px;
					padding: 4px 6px;
				}
				.item.active{
					border-color: #ffe081;
					background-color: #fff0b7;
				}
				
			}
			.select_gg{
				margin: 26px -12px 0;
				padding: 6px 12px;
				color: #999;
				background-color: #f9f9f9;
				display: flex;
				flex-direction: row;
				.lable{
					
				}
				.select_gg_box{
					flex: 1;
					display: flex;
					flex-direction: row;
					flex-wrap: wrap;
					.gg-item{
						color: #333;
						.gg-item-cut{
							margin-left: 3px;
							margin-right: 3px;
						}
					}
					.gg-item:last-child{
						.gg-item-cut{
							display: none;
						}
					}
				}
				
			}
			.bottom{
				position: relative;
				display: flex;
				flex-direction: row;
				margin-top: 12px;
				.price_box{
					flex: 1;
					lign-items: baseline;
					color: #ff582b;
					font-size: 14px;
					position: relative;
					top: 4px;
					.price{
						font-size:  24px;
					}
				}
				.jumpPosition{
					position: absolute;
					right: 16px;
					top: 2px;
				}
				.form-btn-box{
					height: 30px;
					line-height: 30px;
					.add-btn{
						border-radius: 50px;
						background-color: #ffce3c; 
						padding: 0 12px;
						display: flex;
						height: 30px;
						line-height: 30px;
						align-items: center;
						i{
							font-weight: bold;
							font-size: 18px;
							margin-right: 3px;
							margin-left: -4px;
						}
						text{
							
							font-size: 14px;
							font-weight: bold;
							color: #363636;
						}
					}
				}
			}
			
		}
		
		.close{
			position: absolute;
			left: 50%;
			margin-left: -20px;
			bottom: -70px;
			border-radius: 50%;
			height: 40px;
			width: 40px;
			background-color: #fff;
			opacity: 0.7;
			text-align: center;
			line-height: 43px;
			i{
				font-weight: bold;
				font-size: 22px;
			}
		}
	}
</style>
