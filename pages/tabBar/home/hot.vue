<template>
    <view class="uni-container" style="background: #f5f5f5">
		<view class="uni-container-home-banner">
            <view class="uni-list" style="margin-bottom: 70px;">
				<block v-for="(value, index2) in feedList" :key="index2">
					<view class="uni-list-cell" hover-class="uni-list-cell-hover" @click="goDetail(value)">
						<view class="uni-media-list">
							<image class="uni-media-list-logo" :src="value.cover_image" />
							<view class="uni-media-list-body">
								<view class="uni-media-list-text-top">{{ value.name }}</view>
								<view class="uni-media-list-text-bottom">{{ value.brief_info }}</view>
								<view class="uni-media-list-text-pintuan">起送 ${{ value.pintuan }} | 配送 $0</view>
                                <view class="uni-media-list-text-people">
									<text class="uni-media-list-text-people-1">已团{{value.shopping ? value.shopping.length : 10 }}</text>
									<text class="uni-media-list-text-people-2">立即下单</text>
								</view>
							</view>
						</view>
					</view>
				</block>
			</view>
		</view>
	</view>
</template>
<script>
	import dataMock from '@/mock/home.json';

    export default {
        components: {},
        data() {
			return {
                feedList: [],
            }
        },
        onLoad() {
			const dataJson = dataMock.data;
			this.feedList = dataJson.shopList;
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

	.uni-list {
        margin: 0 12px 16px 12px;
        padding-top: 12px;
	}

	.uni-list-cell {
		height: 124px;
		width: 100%;
        margin-bottom: 12px;
        background: #ffffff;
        box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.06);
        border-radius: 4px;
		position: relative;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
	}
	.uni-media-list {
		width: 100%;
		box-sizing: border-box;
		display: flex;
        flex-direction: row;
        padding: 12px;
	}
	.uni-media-list-logo {
		height: 100px;
		width: 100px;
        margin-right: 12px;
        border-radius: 4px;
	}
	.uni-media-list-logo image {
		height: 100%;
		width: 100%;
	}
	.uni-media-list-body {
		height: 100px;
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: start;
		align-items: flex-start;
		overflow: hidden;
	}
	.uni-media-list-text-top {
		height: 17px;
		font-weight: 600;
		font-size: 14px;
		line-height: 17px;
		overflow: hidden;
	}
	.uni-media-list-text-bottom {
		width: 100%;
		line-height: 17px;
		height: 17px;
		font-size: 12px;
		overflow: hidden;
		color: rgba(0, 0, 0, 0.54);
		margin-top: 4px;
	}
	.uni-media-list-text-pintuan {
		width: 100%;
		height: 17px;
		margin-top: 2px;
		font-size: 12px;
		line-height: 17px;
		overflow: hidden;
		color: rgba(0, 0, 0, 0.54);
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

	.uni-media-list-text-people {
		bottom: 0;
		display: flex;
        position: absolute;
        font-size: 12px;
        color: #FFFFFF;
        line-height: 17px;
        margin-bottom: 12px;
        border-radius: 30px;
    }
    
    .uni-media-list-text-people-1 {
        padding: 6px;
        background: #494949;
        border-top-left-radius: 30px;
        border-bottom-left-radius: 30px;
    }

    .uni-media-list-text-people-2 {
        padding: 6px;
        background: #EE5B2D;
        border-top-right-radius: 30px;
        border-bottom-right-radius: 30px;
    }
    
</style>