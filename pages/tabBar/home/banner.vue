<template>
    <view class="uni-container" style="background: #ffffff">
		<view class="uni-container-home-banner">
			<view class="uni-title">锅巴美食街</view>
			<view class="uni-time">Jean   28.10.2020</view>
			<view class="uni-doc">团购，英文名是Group purchase，意思是团体购物，指认识或不认识的消费者联合起来，加大与商家的谈判能力，以求得最优价格的一种购物方式。根据薄利多销的原理，商家可以给出低于零售价格的团购折扣和单独购买得不到的优质服务。团购作为一种新兴的电子商务模式，通过消费者自行组团、专业团购网站、商家组织团购等形式，提升用户与商家的议价能力，并极大程度地获得商品让利，引起消费者及业内厂商、甚至是资本市场关注。</view>
			<view class="uni-doc">一、对于没有多少时间的顾客或者一些要求比较高的顾客是有价格上的好处的（这类顾客在论坛上比较少）。不是每个顾客都会花大量的时间去学习知识，也不是每个顾客都知道那里买东西便宜，很多顾客甚至只逛商场，那么团购的价格的确是比商场便宜了。就是有些顾客逛市场，市场上很多顾客找不到，商家多而杂，你找到好的了是可能要便宜点，你没有找到好的那不是讲了半天价格还要贵点吗？不过，对于这类顾客来说，还是有一个原则，团购的时候不要盲目，连自己要什么都不知道看见团购就去交定金，你起码要知道你到底要什么。
二、对于一天到晚都在网络上学习知识或者有一定时间去了解市场的顾客来说，意义就是你会找建材市场便宜的服务好的商家，不太可能把每个品种找完的，总有你找不到的，找不到的就有团购就可以去参加，虽然不一定便宜，不过也是不贵的，也许这个价格和特价是一样的。</view>
		</view>
	</view>
</template>
<script>
    export default {
        components: {},
        data() {
			return {}
		},
        methods: {}
    };
</script>

<style>
	page {
		display: flex;
		flex-direction: column;
		box-sizing: border-box;
		min-height: 100%;
		height: auto;
	}

	view {
		font-size: 14px;
		line-height: inherit;
	}

	.uni-title {
		font-weight: 500;
		font-size: 20px;
		line-height: 28px;
		color: rgba(0, 0, 0, 0.87);
		margin-left: 12px;
	}
	.uni-time {
		font-size: 14px;
		line-height: 20px;
		margin-left: 12px;
		margin-top: 8px;
		color: rgba(0, 0, 0, 0.54);
	}
	.uni-doc {
		font-size: 14px;
		line-height: 23px;
		color: rgba(0, 0, 0, 0.54);
		margin-top: 20px;
		padding: 0 20px;
	}
</style>