<template>
	<view>
		<view class="header">
			<image class="back" src="../../static/black-back.png" mode="" @click='back'></image>
		</view>
		<container title='基础式'>
			<ls-swiper :list="base_lsit" imgKey="imgUrl" :autoplay='true' height='150' @clickItem="clickItem()" />
		</container>
		<container title='卡片式'>
			<ls-swiper :list="base_lsit" imgKey="imgUrl" :crown="true" :imgShadow='true' height='130' preNextMargin="120"
			 imgRadius="5" />
		</container>
		<container title='链式'>
			<ls-swiper :list="base_lsit" imgKey="imgUrl" imgWidth="95%" preNextMargin="70" height='130'
			 imgRadius="5" />
		</container>
	</view>
</template>

<script>
	import api from '../../api/index.js'
	import container from '../../components/container/index.vue'
	import LsSwiper from '../../components/ls-swiper/index.vue'
	export default {
		components: {
			container,
			LsSwiper
		},
		data() {
			return {
				base_lsit: []
			}
		},
		onLoad() {
			this.getBase()
		},
		methods: {
			getBase() {
				api.base(100).then(res => {
					this.base_lsit = res
				})
			},
			clickItem(data) {
				console.log(data)
			},
			back() {
				uni.redirectTo({
					url: '/pages/index/index',
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.header {
		margin-top: 40rpx;
		display: flex;
		align-items: center;
		height: 80rpx;
		padding-left: 20rpx;
		margin-bottom: 30rpx;

		.back {
			width: 30rpx;
			height: 30rpx;
		}
	}
</style>
