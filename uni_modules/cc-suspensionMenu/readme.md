# cc-suspensionMenu

#### 使用方法 
```使用方法
<!-- scrollShow:是否显示滑动到顶悬浮按钮  color：悬浮按钮背景色  iconList：悬浮菜单弹出数组  @click：点击事件-->
<cc-suspensionMenu :scrollShow="scrollShow" colors="#fa436a" :iconList="iconList"
@click="menuClick"></cc-suspensionMenu>
```

#### HTML代码实现部分
```html
<template>
	<view class="content">

		<!-- scrollShow:是否显示滑动到顶悬浮按钮  color：悬浮按钮背景色  iconList：悬浮菜单弹出数组  @click：点击事件-->
		<cc-suspensionMenu :scrollShow="scrollShow" colors="#fa436a" :iconList="iconList"
			@click="menuClick"></cc-suspensionMenu>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				colors: '#fa436a',

				scrollShow: false, //是否显示悬浮菜单

				iconList: [{

						name: '搜索',
						icon: require('../../static/search.png'),


					},
					{
						name: '客服',
						icon: require('../../static/serve.png'),


					}
				]



			}
		},

		methods: {

			menuClick: function(item) {

				console.log("点击悬浮按钮条目item = " + JSON.stringify(item.name));
				uni.showModal({
					title: '点击悬浮按钮条目',
					content: "点击悬浮按钮条目item = " + JSON.stringify(item.name)
				})
			},

		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;

	}
</style>



```