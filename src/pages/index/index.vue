<template>
	<view class="content">
		<view class="background">
		</view>

		<view class="start">
			<text class="text1">{{text1}}</text>
			<text class="text2">{{text2}}</text>
			<button class="start_button" @click="goTo('/pages/fruits/fruits')">{{button}}</button>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			text1: '水果的秘密！',
			text2: '探索水果背后的生长环境',
			button: '开始游戏'
		}
	},
	onLoad() {

	},
	methods: {
		goTo(url) {
			uni.switchTab({
				url: url
			})
		}
	}
}
</script>

<style>
.content {
	width: 376px;
	height: 812px;
	padding: 0 0;
	display: flex;
	flex-direction: column;
	justify-content: center;
}

.background {
	width: 100%;
	height: 550px;
	display: flex;
	background-image: url("/static/images/startbg.png");
	background-repeat: no-repeat;
	background-size: cover;
}

.start {
	width: 100%;
	height: 264px;
	display: flex;
	flex-direction: column;
	align-items: center;
	background: #F2F2F2;
}

.start .text1 {
	margin-top: 2vh;
	font-family: 'PingFang SC';
	font-style: normal;
	font-weight: 600;
	font-size: 24px;
	color: #303030;
}

.start .text2 {
	font-family: 'PingFang SC';
	font-style: normal;
	font-weight: 400;
	font-size: 16px;
	color: #303030;
}

.start_button {
	margin-top: 3.5vh;
	width: 266px;
	height: 46px;
	font-weight: 700;
	font-family: 'PingFang SC';
	font-style: normal;
	font-size: 18px;
	color: #ffffff;
	background-image: url("/static/images/group.png");
	background-color: transparent;
	background-repeat: no-repeat;
	background-size: cover;
	border-radius: 100px;
}
</style>
