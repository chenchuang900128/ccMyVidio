<template>
	<view class="suspension">
		<view :class="showsever == true ? 'topon' : 'top'">
			<view class="serve" v-for="(item,index) in iconList" :key="index" @tap="jumpServer(item)">

				<image class="imgV" mode="aspectFit" :src="(item.icon)"></image>

			</view>
		</view>
		<view class="bottom">
			<view class="serve" :style="'background-color:' + colors" @tap="onshowsever">

				<image class="imgV" mode="aspectFit" src="./cate.png"></image>

			</view>
			<view class="ontop" :style="'opacity: ' + (scrollShow == true ? '1':'0')" @tap="goTop">
				<image src="./TOP.png" class="imgV"></image>
			</view>
		</view>
	</view>
</template>

<script>
	import Vue from 'vue';
	// mixin技术
	Vue.mixin({
		methods: {
			setData: function(obj, callback) {
				let that = this;
				const handleData = (tepData, tepKey, afterKey) => {
					tepKey = tepKey.split('.');
					tepKey.forEach(item => {
						if (tepData[item] === null || tepData[item] === undefined) {
							let reg = /^[0-9]+$/;
							tepData[item] = reg.test(afterKey) ? [] : {};
							tepData = tepData[item];
						} else {
							tepData = tepData[item];
						}
					});
					return tepData;
				};
				const isFn = function(value) {
					return typeof value == 'function' || false;
				};
				Object.keys(obj).forEach(function(key) {
					let val = obj[key];
					key = key.replace(/\]/g, '').replace(/\[/g, '.');
					let front, after;
					let index_after = key.lastIndexOf('.');
					if (index_after != -1) {
						after = key.slice(index_after + 1);
						front = handleData(that, key.slice(0, index_after), after);
					} else {
						after = key;
						front = that;
					}
					if (front.$data && front.$data[after] === undefined) {
						Object.defineProperty(front, after, {
							get() {
								return front.$data[after];
							},
							set(newValue) {
								front.$data[after] = newValue;
								that.$forceUpdate();
							},
							enumerable: true,
							configurable: true
						});
						front[after] = val;
					} else {
						that.$set(front, after, val);
					}
				});
				isFn(callback) && this.$nextTick(callback);
			}
		}
	});

	export default {
		data() {
			return {
				showsever: false,

			};
		},
		props: {
			scrollShow: { //监听页面滚动
				type: Boolean,
				default: true
			},
			colors: {
				type: String,
				default: '#fa436a'
			},
			iconList: {
				type: Array,

			},

		},
		methods: {
			onshowsever() { //控制top按钮的显示
				let shows = !this.showsever;
				this.setData({
					showsever: shows
				});
			},
			goTop() { //回到顶部
				uni.pageScrollTo({
					scrollTop: 0
				});
			},
			jumpServer(item) { //跳转
				this.$emit("click", item);
			}

		}
	};
</script>
<style scoped lang="scss">
	.imgV {
		width: 22px;
		height: 22px;

		margin: 6px 6px;
	}

	.suspension {
		position: fixed;
		bottom: 12vh;
		right: 3%;
		width: 80upx;
		z-index: 200;
	}

	.suspension .top {
		width: 100%;
		padding-bottom: 20upx;
		transition: all 0.3s;
		transform: scale(0.1) translateY(80%);
		opacity: 0;
		z-index: -100;

		.serve {
			width: 0upx;
			height: 0upx;
			background-color: rgba(0, 0, 0, 0.5);
			color: #fff;
			border-radius: 50%;
			margin-bottom: 20upx;

			display: flex;
			justify-content: center;
			align-items: center;
			align-self: center;


		}
	}

	.topon {
		width: 100%;
		transition: all 0.3s;
		opacity: 1;
		transform: none;
		z-index: 100;

		.serve {
			width: 70upx;
			height: 70upx;
			background-color: rgba(0, 0, 0, 0.5);
			color: #fff;
			border-radius: 50%;
			margin-bottom: 20upx;
		}
	}

	.suspension .bottom .serve {
		width: 70upx;
		height: 70upx;
		background-color: rgba(0, 0, 0, 0.5);
		color: #fff;
		border-radius: 50%;
		margin-bottom: 20upx;
	}

	.suspension .serve:active {
		opacity: 0.8;
	}

	.serve .iconfont {
		font-size: 32upx;
		display: block;
		text-align: center;
		line-height: 70upx;

	}

	.ontop {
		width: 70upx;
		height: 70upx;
		background-color: rgba(0, 0, 0, 0.6);
		border-radius: 50%;
		transition: all 0.3s;
	}

	.ontop:active {
		opacity: 0.8;
	}

	.ontop .top_img {
		width: 45upx;
		height: 55upx;
		display: block;
		margin: 0 auto;
		padding-top: 15upx;
		overflow: hidden;
	}
</style>