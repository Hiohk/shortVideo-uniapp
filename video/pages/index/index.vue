<template>
	<view class="content">
		<view class="fixed_top">
			<view class="main_top">
				<top></top>
			</view>
			<view class="u-demo-block__content title_bar">
				<u-row justify="space-between">
					<u-col span="2" align="center">
						<view class="demo-layout bg-purple top_title people">
							人才
						</view>
						</a>
					</u-col>
					<u-col span="2" align="center">
						<view class="demo-layout bg-purple-light top_title employer">
							雇主
						</view>
					</u-col>
					<u-col span="2" align="center">
						<view class="demo-layout bg-purple title_partner partner">
							合伙人
						</view>
					</u-col>
					<u-col span="2" align="center">
						<view class="demo-layout bg-purple-light top_title recommendation">
							推荐
						</view>
					</u-col>
					<u-col span="2" align="center">
						<view class="demo-layout bg-purple-light top_title focus">
							关注
						</view>
					</u-col>
					<u-col span="2">
						<view class="demo-layout bg-purple-light">
							<u-switch v-model="value" @change="change" activeColor="#5ac725" size="40" class="btn_switch"></u-switch>
						</view>
					</u-col>
				</u-row>
			</view>
		</view>
		
		<view class="main_video">
			<view class="parent_video">
				<view>
					<swiper class="swiper" :indicator-dots="false" :duration="200" :vertical="true" :current="videoIndex" @change="handleSlider" style="height: 1300rpx;">
					    <!-- <cell v-for="(item,index) in dataList" :key="index"> -->
					        <swiper-item v-for="(item,index) in dataList" :key="index">
					            <view class="uni_vdplayer">
					                <!-- <video :id="'myVideo' + index" :ref="'myVideo' + index" class="my_video player-video" :src="item.src" 
					                :controls="false" :loop="true" :show-center-play-btn="false" objectFit="fill">
					                </video> -->
									<video 
										:id="'myVideo' + index" 
										:ref="'myVideo' + index" 
										class="my_video player-video" 
										:src="item.src"
										:muted="item.isplay"
										:controls="false" 
										:loop="true" 
										@play="playIngs(index)"	
										:show-center-play-btn="true" 
										:enable-progress-gesture="false"
										object-fit="fill">
									</video>
									<image
										v-if="!item.playIng"
										:src="item.href" 
										:mode="mode"
										class="cover"
									></image>
					                <!-- 中间播放按钮 -->
					                <view class="vd-cover flexbox" @click="handleClicked(index)"><text v-if="!isPlay" class="iconfont icon-bofang"></text></view>
					            </view>
								
								<!-- <view class="videoHover" @click="tapVideoHover(item.state,$event)">
									<image v-if="item.state=='pause'" class="playState" src="../../static/icons/play.png"></image>
								</view> -->
								<view class="right_bar">
									<view v-if="item.isShowProgressBarTime == false">
										<!-- <u-icon size="100" name="../../static/icons/avator.svg"></u-icon>
										<u-icon size="40" name="../../static/icons/plus.svg" class="plus"></u-icon> -->
										<image class="userAvatar" :src="item.href" mode="aspectFill"></image>
										<u-icon size="40" name="../../static/icons/plus.svg" class="plus"></u-icon>
									</view>
									<view v-if="item.isShowProgressBarTime == false">
										<u-icon v-if="approve" size="55" name="../../static/icons/approve.svg" class="icons" @click="approve=!approve"></u-icon>
										<u-icon v-else size="55" name="../../static/icons/approve_select.svg" class="icons" @click="approve=!approve"></u-icon>
										<text class="like_num1" :class="{'likeNumActive':item.like}">{{item.like}}</text>
									</view>
									<view v-if="item.isShowProgressBarTime == false">
										<u-icon v-if="collect" size="55" name="../../static/icons/collect.svg" class="icons" @click="collect=!collect">4</u-icon>
										<u-icon v-else size="55" name="../../static/icons/collect_select.svg" class="icons" @click="collect=!collect"></u-icon>
										<text class="like_num2" :class="{'likeNumActive':item.like}">{{item.like}}</text>
									</view>
									<view v-if="item.isShowProgressBarTime == false">
										<u-icon size="55" name="../../static/icons/share.svg" class="icons"></u-icon>
										<text class="like_num3" :class="{'likeNumActive':item.like}">{{item.like_n}}</text>
									</view>
									<view v-if="item.isShowProgressBarTime == false">
										<u-icon size="55" name="../../static/icons/comment.svg" class="icons"></u-icon>
										<text class="like_num4" :class="{'likeNumActive':item.like}">{{item.sms_n}}</text>
									</view>
									<view v-if="item.isShowProgressBarTime == false">
										<u-icon size="55" name="../../static/icons/talk.svg" class="icons"></u-icon>
									</view>
								</view>
					        </swiper-item>
					    <!-- </cell> -->
					</swiper>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	let timer = null;
	export default {
		data() {
			return {
				value: true,
				src: 'http://pic2.sc.chinaz.com/Files/pic/pic9/202002/hpic2119_s.jpg',
				approve: true,
				collect: true,
				dataList: [],
				videoIndex: 0,
				// vlist: videoJson,
				isPlay: true,    //当前视频是否播放中
				clickNum: 0,   //记录点击次数
				mode: 'aspectFit',
				k:0,
				touchNum: 0,
				ProgressBarOpacity: 0.7,
				dotWidth: 0
			}
		},
		onLoad(option) {
			console.log("onload");
			this.videoIndex = parseInt(option.index);
			this.videoContext = uni.createVideoContext('myVideo');
		},
		created() {
			this.getVideo();
		},
		methods: {
			change(e) {
				console.log("change",e);
			},
			getVideo() {
				uni.request({
					url: "https://bdb24c6d-8c19-4f80-8e7e-c9c9f037f131.bspapp.com/video",
					method: "POST",
					data: {
						info: 'get_video'
					},
					success: res => {
						uni.hideLoading();
						let msg = res.data.data;
						for(let i = 0; i < msg.length; i++) {
							this.dataList.push(msg[i]);
							this.dataList.forEach(item=>{
								item.flag = false;
							})
						}
						console.log("视频数据-->",this.dataList);
					}
				})
			},
			videoErrorCallback: function(e) {
			    uni.showModal({
			        content: e.target.errMsg,
			        showCancel: false
			    })
			},
			playIngs(index) {
				
			},
			// 滑动切换
			handleSlider(e) {
			    let curIndex = e.detail.current
			    if(this.videoIndex >= 0){
			        // this.dataList[this.videoIndex].pause();
			        // this.dataList[this.videoIndex].seek(0);
			        this.isPlay = false;
			    }
			    if(curIndex === this.videoIndex + 1) {
			        // this.dataList[this.videoIndex + 1].play()
			        this.isPlay = true;
			    }else if(curIndex === this.videoIndex - 1) {
			        // this.dataList[this.videoIndex - 1].play()
			        this.isPlay = true;
			    }
			    this.videoIndex = curIndex;
			},
			// 播放
			play(index) {
			    // this.dataList[index].play();
			    this.isPlay = true;
			},
			// 暂停
			pause(index) {
			    // this.dataList[index].pause();
			    this.isPlay = false;
			},
			// 点击视频事件
			handleClicked(index) {
			    if(timer){
			        clearTimeout(timer);
			    }
			    this.clickNum++;
			    timer = setTimeout(() => {
			        if(this.clickNum >= 2){
			            console.log('双击视频');
			        }else{
			            console.log('单击视频');
			            if(this.isPlay){
			                this.pause(index);
			            }else{
			                this.play(index);
			            }
			        }
			        this.clickNum = 0;
			    }, 300)
			},
			tapVideoHover(state,event){
				this.dataList[this.k].isShowimage = false;
				this.dataList[this.k].isShowProgressBarTime = false;
				this.ProgressBarOpacity = 0.5;
				this.dotWidth = 0;
				console.log('state--',state);
				this.touchNum++;
			}
		}
	}
</script>

<style lang="scss">
	.content {
		width: 750rpx;
		background-color: black;
	}
	.title_partner {
		color: #F8E71C;
		font-size: 35rpx;
		padding: 16rpx 5rpx;
	}
	.top_title {
		color: #fff;
		font-size: 35rpx;
		padding: 16rpx 5rpx;
	}
	.top_title::after, .title_partner::after {
	  content: '';
	  width: 50%;
	  height: 1px;
	  display: block;
	  margin: 10rpx auto;
	  border-bottom: 2px solid black;
	  padding: 1px;
	}
	.recommendation {
		font-size: 40rpx;
		font-weight: 600;
	}
	.recommendation:after {
	  content: '';
	  width: 50%;
	  height: 1px;
	  display: block;
	  margin: 10rpx auto;
	  border-bottom: 2px solid #fff;
	  padding: 1px;
	}
	.title_bar {
		height: 90rpx;
		background-color: black;
	}
	.main_top {
		padding-top: 40rpx;
	}
	.fixed_top {
		top: 90rpx;
		left: 0;
		position: fixed;
	}
	.cover {
		position: absolute;
		width: 750rpx;
		height: 900rpx;
	}
	.right_bar {
		top: 350rpx;
		right: 10rpx;
		position: absolute;
		// display: flex;
		flex-direction: column;
		// background-color: pink;
		view {
			width: 100rpx;
			height: 120rpx;
			padding: 0 10rpx;
			text-align: center;
			// border: 1px solid red;
		}
		.plus {
			top: 52rpx;
			right: -38rpx;
			position: absolute;
		}
		.icons {
			margin: -10rpx 0 0 20rpx;
		}
	}
	.userAvatar{
		border-radius: 500%;
		margin: 16rpx 0 0 10rpx;
		border-style: solid;
		border-width: 2px;
		border-color: #ffffff;
		width: 90rpx;
		height: 90rpx;
	}
	.btn_switch {
		margin-bottom: 25rpx;
	}
	.my_video {
		margin-top: 210rpx;
		width: 750rpx;
		height: 900rpx;
		position: relative;
	}
	.parent_video {
		position: relative;
	}
	.like_num1 {
		top: 200rpx;
		right: 45rpx;
		position: absolute;
		color: #FFFFFF; 
		font-size: 14rpx; 
		text-align: center; 
		font-weight: bold;
	}
	.like_num1,  .like_num2, .like_num3, .like_num4{
		color: #FFFFFF; 
		font-size: 14rpx; 
		text-align: center; 
		font-weight: bold;
	}
	.like_num1 {
		top: 200rpx;
		right: 45rpx;
		position: absolute;
	}
	.like_num2 {
		top: 300rpx;
		right: 45rpx;
		position: absolute;
	}
	.like_num3 {
		top: 420rpx;
		right: 45rpx;
		position: absolute;
	}
	.like_num4 {
		top: 520rpx;
		right: 45rpx;
		position: absolute;
	}
</style>
