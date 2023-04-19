<template>
	<view class="topbar">
		<view class="u-demo-block__content">
			<u-row justify="space-between">
				<u-col span="1" align="center">
					<view class="demo-layout bg-purple">
						<u-icon size="40" name="../../static/icons/search.svg"></u-icon>
					</view>
				</u-col>
				<u-col span="2" align="center">
					<view class="demo-layout bg-purple-light">
						<u-icon size="70" name="../../static/icons/live.png"></u-icon>
					</view>
				</u-col>
				<u-col span="2" align="center">
					<view class="demo-layout bg-purple">
						<u-icon size="70" name="../../static/icons/activity.png"></u-icon>
					</view>
				</u-col>
				<u-col span="2" align="center">
					<view class="demo-layout bg-purple-light">
						<u-icon size="70" name="../../static/icons/AI.png"></u-icon>
					</view>
				</u-col>
				<u-col span="8">
					<view>
					</view>
				</u-col>
			</u-row>
		</view>
	</view>
</template>

<script>
	export default {
		name: "top",
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss">
	.topbar {
		width: 750rpx;
		background-color: black;
	}
	// .wrap {
	//     padding: 12px;
	// }

	// .demo-layout {
	//     height: 25px;
	//     border-radius: 4px;
	// }

	// .bg-purple {
	//     background: #CED7E1;
	// }

	// .bg-purple-light {
	//     background: #e5e9f2;
	// }

	// .bg-purple-dark {
	//     background: #99a9bf;
	// }
</style>
