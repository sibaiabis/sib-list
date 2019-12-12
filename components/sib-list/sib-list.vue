<template>
	<view class="">
		<!-- refresh -->
		<view class="">
			<view class='refreshBox' :style="isTranform">
				<view class='refresh' :style="isZoom" :class="isEnd==2?'animationSmall':''">
					<!-- 下拉样式 -->
					<view class='refreshText' v-if="isEnd == 0">{{loadText}}</view>
					<!-- 加载中样式，可自定义 -->
					<view class='refreshCirle animation' v-if="isEnd == 1"></view>
					<!-- 加载完成样式 -->
					<image class='iconRefreshed' src='../../static/sib-list/iconRefreshed.png' v-if="isEnd==2"></image>
				</view>
			</view>
		</view>

		<!-- list -->
		<view class="">
			<scroll-view scroll-y="true" :style="[{height: scrollHeight}]" @scrolltolower="scrolltolower" @scroll="scroll"
			 scroll-with-animation :scroll-into-view="topView">
				<!-- 回到顶部定位点 -->
				<view id="topId" class=""></view>
				<!-- 列表数据 -->
				<slot name="sibScrollList" v-if="!isNoList" />
				<!-- 暂无数据 -->
				<view class='noCard' v-if="isNoList">{{noListText}}</view>
				<!-- 底部提示 -->
				<view class="floter" v-if="!isNoList">{{floterText}}</view>
			</scroll-view>
		</view>

		<!-- top -->
		<view class="">
			<!-- 回到顶部按钮 (fixed元素,需写在scroll-view外面,防止滚动的时候抖动)-->
			<image v-if="isUseTop && isShowToTop" class="totop" :class="{'fade-in':isShowToTop}" src="../../static/sib-list/totop.png"
			 @click="toTopClick" />
		</view>
	</view>
</template>

<script>
	// 感谢你使用此组件，觉得不错的话去github点个Star吧~
	// https://github.com/gameruleCEO/sib-list
	export default {
		name: 'sib-loading',
		props: {
			isTop: {
				type: [Number, String],
				default: 1
			},
			loadText: {
				type: [String, String],
				default: '松开刷新'
			},
			scrollHeight: {
				type: [String, String],
				default: '500px'
			},
			isNoList: {
				type: [Boolean, String],
				default: false
			},
			noListText: {
				type: [String, String],
				default: '暂无数据...'
			},
			floterText: {
				type: [String, String],
				default: '数据加载中...'
			},
			isUseTop: {
				type: [Boolean, String],
				default: true
			},
			isGtHeight: {
				type: [Number, String],
				default: 1000
			}
		},
		data() {
			return {
				isTranf: 0,
				touchStart: '',
				touchMove: '',
				isEnd: 0,
				isShowToTop: false,
				topView: ''
			};
		},

		methods: {
			// 刷新开始
			refreshStart: function(e) {
				if (this.isEnd == 0 && this.isTop == 1) {
					this.touchStart = e.changedTouches[0].pageY
				}
			},
			// 刷新移动
			refreshMove: function(e) {
				if (this.isEnd == 0 && this.isTop == 1) {
					var touchStart = this.touchStart,
						oldMove = this.touchMove,
						newMove = e.changedTouches[0].pageY
					if (touchStart <= newMove) {
						var isTranf = touchStart > newMove ? 0 : newMove - touchStart
						this.isTranf = isTranf
						this.touchMove = e.changedTouches[0].pageY
					}
				} else {
					this.isTranf = 0
					this.isEnd = 0
					this.touchStart = 9999
				}
			},
			// 刷新结束
			refreshEnd: function(e) {
				var that = this
				if (this.isEnd == 0 && this.isTop == 1) {
					if (this.isTranf >= 90) {
						this.isTranf = 125
						this.isEnd = 1
						this.$emit('isRefresh');
					} else {
						this.isTranf = 0
					}
				}
			},
			// 结束整个刷新过程
			endAfter: function() {
				this.isEnd = 2
				setTimeout(() => {
					this.isTranf = 0
					this.isEnd = 0
				}, 600)
			},
			// 触底
			scrolltolower: function(e) {
				this.$emit('scrolltolowerFn');
			},
			// 回到顶部
			toTopClick: function() {
				this.topView = ''
				setTimeout(() => {
					this.topView = 'topId'
				}, 10)
			},
			// 移动
			scroll: function(e) {
				if(!this.isUseTop) {
					return
				}
				if (e.detail.scrollTop > this.isGtHeight) {
					this.isShowToTop = true
				} else {
					this.isShowToTop = false
				}
			}
		},
		computed: {
			isTranform: function() {
				var isTranf = this.isTranf > 150 ? 150 : this.isTranf
				var isTemp = `transform: translateY(${isTranf- 60}px);`
				return isTemp
			},
			isZoom: function() {
				var isTranf = this.isTranf > 125 ? 125 : this.isTranf
				var isTemp = `zoom:${isTranf/125};`
				return isTemp
			}
		}

	}
</script>

<style>
	.refreshBox {
		margin: 0 auto;
		width: 100%;
		height: 100upx;
		overflow: hidden;
		display: flex;
		align-items: center;
		justify-content: center;
		max-height: 300upx;
		position: fixed;
		z-index: 999;
		top: 0;
		left: 0;
		transform: translateY(-100upx);
	}

	.animationSmall {
		animation: small 1.1s both;
	}

	@keyframes small {
		0% {
			transform: scale(1)
		}

		20% {
			transform: scale(1.4)
		}

		100% {
			transform: scale(0)
		}
	}

	.refreshText {
		width: 150upx;
		height: 26upx;
		font-size: 24upx;
		line-height: 26upx;
		text-align: center;
		border-radius: 26upx;
	}

	.refresh {
		min-width: 50upx;
		display: flex;
		align-items: center;
		justify-content: center;
		height: 50upx;
		background: #FFFFFF;
		box-shadow: 0 0 16upx 0 rgba(0, 0, 0, 0.10);
		border-radius: 50upx;
	}

	.refreshCirle {
		width: 26upx;
		height: 26upx;
		border-radius: 50%;
		display: inline-block;
		position: relative;
		border: 6upx solid black;
		border-bottom-color: transparent;
		border-top-color: transparent;
	}

	.animation {
		animation: rotate 1.1s infinite;
		animation-timing-function: cubic-bezier(0.3, 1.65, 0.7, -0.65);
	}

	@keyframes rotate {
		0% {
			transform: rotate(0deg);
		}

		100% {
			transform: rotate(360deg);
		}
	}

	.true {
		color: black;
	}

	.iconRefreshed {
		width: 34upx;
		height: 34upx;
	}

	.floter {
		width: 100%;
		height: 100upx;
		opacity: 0.5;
		text-align: center;
	}

	.noCard {
		width: 90%;
		height: 200upx;
		margin: auto;
		background-color: white;
		display: flex;
		align-items: center;
		justify-content: center;
		color: #999999;
		box-shadow: 0 0 10upx 0 rgba(0, 0, 0, 0.10);
		border-radius: 10upx;
	}

	/* 回到顶部的按钮 */
	.totop {
		z-index: 9990;
		position: fixed !important;
		/* 避免编译到H5,在多mescroll中定位失效 */
		right: 20upx;
		bottom: 120upx;
		width: 72upx;
		height: 72upx;
		border-radius: 50%;
		opacity: 1;
	}

	.fade-in {
		-webkit-animation: mescrollFadeIn .3s linear forwards;
		animation: mescrollFadeIn .3s linear forwards;
	}
</style>
