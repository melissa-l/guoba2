<template>
	<!-- <view class="swiper-item-menu-detail-price-add">
		<img v-if="rowData.number" src="/static/subtraction.png" @click="del(rowData)">
		<text v-if="rowData.number">{{rowData.number}}</text>
		<img src="/static/add.png" @click="add(rowData)">
	</view> -->

	<view class="uni-numbox">
		<view @click="_calcValue('minus')" class="uni-numbox__minus">
			<img src="/static/subtraction.png" class="uni-numbox--text" :class="{ 'uni-numbox--disabled': inputValue <= min || disabled }" />
		</view>
		<input :disabled="disabled" @blur="_onBlur" class="uni-numbox__value" type="number" v-model="inputValue" />
		<view @click="_calcValue('plus')" class="uni-numbox__plus">
			<img src="/static/add.png" class="uni-numbox--text" :class="{ 'uni-numbox--disabled': inputValue >= max || disabled }"/>
		</view>
	</view>
</template>
<script>
	export default {
		name: "hxNumberBox",
		props: {
			value: {
				type: [Number, String],
				default: 0
			},
			min: {
				type: Number,
				default: 0
			},
			max: {
				type: Number,
				default: 100
			},
			step: {
				type: Number,
				default: 1
			},
			disabled: {
				type: Boolean,
				default: false
			},
			rowData: {
				type: Object,
				default: ()=>{
					return {}
				}
			},
			clickTime: {
				type: Number,
				default: 0
			}
			
		},
		data() {
			return {
				inputValue: 0,
				addStaus: true,
			};
		},
		watch: {
			value(val) {
				this.inputValue = +val;
			},
			inputValue(newVal, oldVal) {
				if (+newVal !== +oldVal) {
					//this.$emit("change", newVal,this.rowData);
				}
				/* if(+newVal > +oldVal){
					
				} */
			}
		},
		created() {
			this.inputValue = +this.value;
		},
		methods: {
			_calcValue(type) {
				let that = this;
				if (this.disabled) {
					return;
				}
			
				const scale = this._getDecimalScale();
				let value = this.inputValue * scale;
				let step = this.step * scale;
				
				
				if (type === "minus") {
					this.$emit("lessChange", this.inputValue,this.rowData);
					value -= step;
					if (value < this.min) {
						return;
					}
					if(value > this.max){
						value = this.max
					}
				} else if (type === "plus") {
					this.$emit("addChange", this.inputValue,this.rowData);
					if(that.clickTime > 0){
						if(!this.addStaus){
							return;
						}else{
							this.addStaus = false;
							setTimeout(()=>{
								that.addStaus = true;
							},that.clickTime)
						}
					}
					value += step;
					if (value > this.max) {
						return;
					}
					if(value < this.min){
						value = this.min
					}
				}

				this.inputValue = String(value / scale);
				this.$emit("change", this.inputValue,this.rowData);
			},
			_getDecimalScale() {
				let scale = 1;
				// 浮点型
				if (~~this.step !== this.step) {
					scale = Math.pow(10, (this.step + "").split(".")[1].length);
				}
				return scale;
			},
			_onBlur(event) {
				let value = event.detail.value;
				if (!value) {
					// this.inputValue = 0;
					return;
				}
				value = +value;
				if (value > this.max) {
					value = this.max;
				} else if (value < this.min) {
					value = this.min;
				}
				this.inputValue = value;
			}
		}
	};
</script>
<style scoped>
	/* #ifdef APP-NVUE */
	/* #endif */

	.uni-numbox {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		height: 24px;
		align-items: center;
	}

	.uni-numbox__value {
		background-color: #ffffff;
		width: 24px;
		height: 24px;
		text-align: center;
		font-size: 16;
		background: #f5f5f5;
		border-radius: 5px;
		margin: 0 4px;
		/* border-width: 1rpx;
		border-style: solid;
		border-color: #e5e5e5;
		border-left-width: 0;
		border-right-width: 0; */
	}

	.uni-numbox__minus {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		align-items: center;
		justify-content: center;
		width: 24px;
		height: 24px;
		/* line-height: $box-line-height;
 */
		/* text-align: center;
 */
		font-size: 20px;
		color: #333;
		/* background-color: #f8f8f8;
		border-width: 1rpx;
		border-style: solid;
		border-color: #e5e5e5;
		border-top-left-radius: 3px;
		border-bottom-left-radius: 3px;
		border-right-width: 0; */
	}

	.uni-numbox__plus {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		align-items: center;
		justify-content: center;
		width: 24px;
		height: 24px;
		/* border-width: 1rpx;
		border-style: solid;
		border-color: #e5e5e5;
		border-top-right-radius: 3px;
		border-bottom-right-radius: 3px; */
		/* background-color: #f8f8f8; */
		/* border-left-width: 0; */
	}

	.uni-numbox--text {
		color: #333;
		width: 24px;
		height: 24px;
	}

	.uni-numbox--disabled {
		color: #c0c0c0;
	}
</style>