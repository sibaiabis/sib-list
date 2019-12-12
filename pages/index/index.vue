<template>
	<view class="">
		<!-- 顶部固定 -->
		<view class="fixed">
			我是顶部固定部分
		</view>

		<!-- 中间数据 -->
		<!-- @touchstart="touchstart" @touchmove="touchmove" @touchend="touchend" 此三个方法必须写 -->
		<view class="list" @touchstart="touchstart" @touchmove="touchmove" @touchend="touchend">
			<!-- ref="sibList" @isRefresh='isRefresh' @scrolltolowerFn="scrolltolowerFn" 此三个必须写 -->
			<sib-list ref="sibList" @isRefresh='isRefresh' @scrolltolowerFn="scrolltolowerFn">
				<!-- 内部block可自定义 -->
				<block class="" slot="sibScrollList">
					<view class="" v-for="(item, index) in list" :key="index">
						<view class="" style="height: 100px;">{{item}}</view>
					</view>
				</block>
			</sib-list>
		</view>

		<!-- 底部固定 -->
		<view class="fixed">
			我是底部固定部分
		</view>
	</view>
</template>

<script>
	import sibList from '@/components/sib-list/sib-list.vue'

	export default {
		data() {
			return {
				list: [
					'原始数据1', '原始数据2', '原始数据3', '原始数据4',
					'原始数据5', '原始数据6', '原始数据7', '原始数据8'
				]
			}
		},
		components: {
			sibList
		},
		methods: {
			// 刷新touch监听
			touchstart: function(e) {
				this.$refs.sibList.refreshStart(e);
			},
			touchmove: function(e) {
				this.$refs.sibList.refreshMove(e);
			},
			touchend: function(e) {
				this.$refs.sibList.refreshEnd(e);
			},
			isRefresh: function() {
				const _this = this
				setTimeout(() => {
					uni.showToast({
						icon: 'success',
						title: '刷新成功,数据恢复初始值'
					})
					const defaultData = [
						'初始数据1', '初始数据2', '初始数据3', '初始数据4',
						'初始数据5', '初始数据6'
					]
					_this.list = defaultData
					// 刷新结束调用
					this.$refs.sibList.endAfter()
				}, 1000)
			},
			scrolltolowerFn: function() {
				uni.showLoading({
					title: '加载中...',
					mask: true
				})
				// 模拟请求
				const _this = this
				setTimeout(() => {
					// 请求成功
					const newData = [
						'新数据1', '新数据2', '新数据3', '新数据4'
					]
					_this.list = _this.list.concat(newData)
					console.log(_this.list)
					uni.hideLoading()
				}, 1000)
			}
		}
	}
</script>

<style>
	.fixed {
		width: 100%;
		height: 100px;
		text-align: center;
		line-height: 100px;
	}

	.list {
		border: 1px solid #0ff;
		text-align: center;
		line-height: 100px;
	}
</style>
