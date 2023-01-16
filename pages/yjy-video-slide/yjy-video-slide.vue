<template>
	<view class="widget-video">
		<view class="scroll-video-box" @touchstart="touchStart" @touchmove="touchMove" @touchend="touchEnd" :style="{'marginTop':marginTop+'px','transition':transition}">
			<view v-for="(item, index) in videoList" :key="index" style="width: 100vw;height: 100vh;">
				<i @click="dbClickAnimation" class="iconfont iconxihuan fabulous-item" :style="{'top':fabulous.top,'left':fabulous.left,'opacity':fabulous.opacity,'transform':fabulous.transform}"
				 v-for="(fabulous, fabulousIndx) in fabulousArr" :key="fabulousIndx"></i>
				<i :class="['iconfont iconbofang btn-play',currentStatus == 'pause' && currentIndex == index ? 'show' : '']" @click="pauseVideo"></i>
				<video @click="pauseVideo" :id="item.id" class="fullscreen-video" :src="item.video_url" :poster="item.poster_url"
				 controls :show-progress="false" :show-fullscreen-btn="false" :show-play-btn="false" :loop="true" :autoplay="index == 0"
				 :show-center-play-btn="false">
				</video>
				<view class="video-info">
					<view class="atavar-box">
						<image class="atavar-img" src="../../static/logo.png" mode="widthFix"></image>
						<view class="add-follow-btn">
							<i class="iconfont iconjia1 inline-block"></i>
						</view> 
					</view>
					<view class="icon-box" @click="toggleFabulous(item, index)">
						<i :class="['iconfont iconxihuan icon-btn', item.isFabulous ? 'color-red' : '']"></i>
						<view class="count-text">3731</view>
					</view>
					<view class="icon-box" @click="commontAdd">
						<i class="iconfont iconIMliaotian-shixin icon-btn"></i>
						<view class="count-text">25</view>
					</view>
					<view class="icon-box">
						<i class="iconfont iconfenxiang icon-btn"></i>
						<view class="count-text">13</view>
					</view>
				</view>
				<view class="video-title">
					<view class="user-name">@Yjyyyyyy</view>
					<view class="video-content">
						如不想根据屏幕宽度缩放，则应该使用 px 单位。如果开发者在字体或高度中也使用了 rpx ，那么需注意这样的写法意味着随着屏幕变宽，字体会变大、高度会变大。如果你需要固定高度，则应该使用 px 。
					</view>
				</view>
			</view>
		</view>
		<view :class="['commont-box',commontShow ? 'active' : '']">
			<view class="commont-title">5578条评论</view>
			<view class="commont-list">
				<view v-for="(item, index) in 20" :key="index">
					<view class="comment-panel">
						<image class="first-user" src="../../static/logo.png" mode="widthFix"></image>
						<view class="first-comment">
							<view class="comment-name">Yjyyyyyy</view>
							<view class="comment-content">
								这是一条测试评论这是一条测试评论这是一条测试评论这是一条测试评论这是一条测试评论这是一条测试评论
							</view>
							<view class="time-box">
								<text class="inline-block">昨天22:00</text>
								<view class="inline-block reply-btn">回复</view>
							</view>
						</view>
						<view class="fabulous-box">
							<i class="iconfont iconxihuan fabulous-btn"></i>
							<view class="fabulous-text">3731</view>
						</view>

					</view>
					<view style="padding-left: 70rpx;">
						<view class="comment-panel">
							<image class="first-user" src="../../static/logo.png" mode="widthFix"></image>
							<view class="first-comment">
								<view class="comment-name">Yjyyyyyy</view>
								<view class="comment-content">
									这是一条测试评论这是一条测试评论这是一条测试评论这是
								</view>
								<view class="time-box">
									<text class="inline-block">昨天22:00</text>
									<view class="inline-block reply-btn">回复</view>
								</view>
							</view>
							<view class="fabulous-box">
								<i class="iconfont iconxihuan fabulous-btn"></i>
								<view class="fabulous-text">3731</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 双击点赞记录
				fabulousArr: [],
				// 记录点击时间判断单击还是双击，单击暂停，双击点赞
				clickTimer: null,
				// 记录点击时间
				clickTime: 0,
				// 屏幕高度
				windowHeight: uni.getSystemInfoSync().windowHeight || window.innerHeight,
				// 动画效果
				transition: 'none',
				// 触摸开始值
				startY: 0,
				// 触摸移动值
				moveY: 0,
				// 滑动开始时间
				startTime: 0,
				// 向上滑动距离
				marginTop: 0,
				// 每次向上滑动的距离
				fixMarginTop: 0,
				// 滑动方向
				moveDirection: '',
				// 当前轮播的index
				currentIndex: 0,
				// 当前页的视频对象
				currentVideo: null,
				// 视频状态：播放/暂停
				currentStatus: 'play',
				videoList: [{
					id: '1',
					isFabulous: 1,
					video_url: 'https://v-cdn.zjol.com.cn/280443.mp4',
					poster_url: 'https://img-my.csdn.net/uploads/201407/26/1406383299_1976.jpg'
				}, {
					id: '2',
					isFabulous: 0,
					video_url: 'https://v-cdn.zjol.com.cn/276982.mp4',
					poster_url: 'https://img-my.csdn.net/uploads/201407/26/1406383291_6518.jpg'
				}, {
					id: '3',
					isFabulous: 1,
					video_url: 'https://v-cdn.zjol.com.cn/276984.mp4',
					poster_url: 'https://img-my.csdn.net/uploads/201407/26/1406383291_8239.jpg'
				}, {
					id: '4',
					isFabulous: 0,
					video_url: 'https://v-cdn.zjol.com.cn/276985.mp4',
					poster_url: 'https://img-my.csdn.net/uploads/201407/26/1406383290_9329.jpg'
				}, {
					id: '5',
					isFabulous: 1,
					video_url: 'https://v-cdn.zjol.com.cn/276986.mp4',
					poster_url: 'https://img-my.csdn.net/uploads/201407/26/1406383290_1042.jpg'
				}, ],
				// 是否显示评论
				commontShow: false,
				// 评论列表
				commentList: [

				]
			};
		},
		onReady() {
			// 初始化拿到第一个视频对象
			this.currentVideo = uni.createVideoContext(this.videoList[0].id, this);
		},
		methods: {
			// 触摸开始
			touchStart(e) {
				this.transition = 'none';
				this.startY = e.touches[0].pageY;
				this.startTime = new Date().getTime();
			},
			// 触摸滑动
			touchMove(e) {
				// 如果评论面板打开，禁止滑动
				if (this.commontShow) {
					return;
				}
				this.moveY = e.touches[0].pageY;
				if (this.moveY > this.startY) {
					// 返回上一个视频
					let range = this.fixMarginTop + this.moveY - this.startY;
					if (this.currentIndex == 0) {
						this.marginTop = range > 100 ? 100 : range;
					} else {
						this.marginTop = range;
					}
					this.moveDirection = 'up';
				} else {
					// 查看下一个视频
					let range = this.fixMarginTop - this.startY + this.moveY;
					if (this.currentIndex == this.videoList.length - 1) {
						// 最后一个视频的位置
						let lastVideo = -this.currentIndex * this.windowHeight + -50;
						this.marginTop = range < lastVideo ? lastVideo : range;
					} else {
						this.marginTop = range;
					}
					this.moveDirection = 'down';
				}
			},
			// 触摸结束
			touchEnd(e) {
				this.transition = 'all .2s';
				let millisecond = new Date().getTime() - this.startTime;
				let condition1 = this.moveY > 0 && Math.abs(this.moveY - this.startY) > 50 && millisecond < 500;
				let condition2 = this.moveY > 0 && Math.abs(this.moveY - this.startY) > this.windowHeight / 3;
				if (condition1 || condition2) {
					if (this.moveDirection == 'up') {
						// 返回上一个
						if (this.currentIndex == 0) {
							this.marginTop = 0;
						} else {
							this.marginTop = this.fixMarginTop + this.windowHeight;
							this.currentIndex = this.currentIndex - 1;
							this.videoPlayChange();
						}
					} else {
						// 查看下一个
						if (this.currentIndex == this.videoList.length - 1) {
							this.marginTop = this.fixMarginTop;
						} else {
							this.marginTop = this.fixMarginTop - this.windowHeight;
							this.currentIndex = this.currentIndex + 1;
							this.videoPlayChange();
						}

					}
				} else {
					this.marginTop = this.fixMarginTop;
				}
				this.fixMarginTop = this.marginTop;
				this.startY = 0;
				this.moveY = 0;
			},
			// 滑动切换
			videoPlayChange() {
				this.stopOtherVideo();
				let video = uni.createVideoContext(this.videoList[this.currentIndex].id, this);
				video.play();
				this.currentStatus = 'play';
				this.currentVideo = video;
			},
			// 暂停其他视频
			stopOtherVideo() {
				this.videoList.map(v => {
					let video = uni.createVideoContext(v.id, this);
					// video.stop();
					video.seek(1);
					video.pause();
				})
			},
			// 暂停/播放视频/双击点赞事件
			pauseVideo(e) {
				if (this.commontShow) {
					this.commontShow = false;
					return;
				}
				clearTimeout(this.clickTimer);
				let currentTime = new Date().getTime();
				let timeRange = currentTime - this.clickTime;
				if (timeRange < 300) {
					// 双击事件
					this.dbClickAnimation(e);

				} else {
					// 单击事件
					this.clickTimer = setTimeout(() => {
						e.preventDefault();
						e.stopPropagation();
						if (this.currentStatus == 'play') {
							this.currentVideo.pause();
							this.currentStatus = 'pause';
						} else {
							this.currentVideo.play();
							this.currentStatus = 'play';
						}
					}, 300);
				}
				this.clickTime = new Date().getTime();
			},
			// 双击点赞动画
			dbClickAnimation(e) {
				let deg = Math.round(Math.random() * 40 + 5); 
				this.fabulousArr.push({
					left: e.detail.x - 25 + 'px',
					top: e.detail.y - 25 + 'px',
					transform: 'rotate(' + (deg % 2 == 0 ? deg : -deg) + 'deg)' 
				})
				console.log(this.fabulousArr);
				let index = this.fabulousArr.length - 1;
				setTimeout(() => {
					this.$set(this.fabulousArr, index, Object.assign(this.fabulousArr[index], {
						opacity: 0,
						transform: 'scale(3) ' + this.fabulousArr[index].transform
					}))
				}, 500);
				setTimeout(() => {
					this.fabulousArr.shift();
				}, 1000);
				if(!this.videoList[this.currentIndex].isFabulous){ 
					this.$set(this.videoList,this.currentIndex,Object.assign(this.videoList[this.currentIndex],{isFabulous:1}))
				}
			},
			// 点赞切换
			toggleFabulous(item, index){
				this.$set(this.videoList, index, Object.assign(item,{
					isFabulous: item.isFabulous ? 0 : 1
				}));
			},
			// 评论
			commontAdd() {
				this.commontShow = true;
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './yjy-video-slide.css';
	// 喜欢按钮动画
	.iconxihuan{
		transition: all .3s;
	}
	.iconxihuan:active{
		transform: scale(2);
	}
	// 双击点赞样式
	.fabulous-item {
		position: absolute;
		z-index: 99999;
		width: 100rpx;
		height: 100rpx;
		color: red;
		font-size: 100rpx;
		transition: opacity .5s, transform .5s;
		opacity: 1;
	}

	// 评论框
	.commont-box {
		position: absolute;
		z-index: 9;
		left: 0;
		bottom: 0;
		width: 100%;
		height: 60%;
		overflow-y: auto;
		background-color: #222;
		border-top-left-radius: 20rpx;
		border-top-right-radius: 20rpx;
		transition: all .3s;
		transform: translateY(100%);
		display: flex;
		flex-direction: column;

		.commont-title {
			text-align: center;
			color: white;
			font-size: 24rpx;
			margin: 20rpx 0;
			font-weight: bold;
		}

		.commont-list {
			flex: 1;
			overflow-y: auto;

			.comment-panel {
				margin: 30rpx 40rpx;
				display: flex;

				.first-user {
					width: 50rpx;
					height: 50rpx;
					border-radius: 100%;
				}

				.first-comment {
					flex: 1;
					margin: 0rpx 20rpx;

					.comment-name {
						font-size: 24rpx;
						font-weight: bold;
						color: #999;
					}

					.comment-content {
						color: white;
						font-size: 26rpx;
						margin-top: 10rpx;
					}

					.time-box {
						margin-top: 10rpx;
						font-size: 24rpx;
						color: #999;

						.reply-btn {
							margin-left: 20rpx;
						}
					}
				}

				.fabulous-box {
					text-align: center;

					.fabulous-btn {
						font-size: 36rpx;
						color: #666;
					}

					.fabulous-text {
						font-size: 24rpx;
						color: #999;
					}
				}
			}
		}
	}

	.commont-box.active {
		transform: translateY(0);
	}

	.widget-video {
		width: 100%;
		height: 100%;
		overflow: hidden;

		.btn-play {
			font-size: 80rpx;
			color: white;
			position: absolute;
			top: 50%;
			left: 50%;
			transition: all .3s;
			transform: translateX(-50%) translateY(-50%) scale(1.5);
			z-index: 8;
			opacity: 0;
		}

		.btn-play.show {
			opacity: 0.5;
			transform: translateX(-50%) translateY(-50%) scale(1);

		}
	}

	.fullscreen-video {
		width: 100%;
		height: 100%;
		position: relative;
	}

	.video-info {
		width: 100rpx;
		position: absolute;
		right: 20rpx;
		bottom: 100rpx;

		.atavar-box {
			background-color: white;
			padding: 6rpx;
			border-radius: 100%;
			height: 100rpx;
			margin-bottom: 64rpx;

			.atavar-img {
				width: 100%;
				display: block;
				border-radius: 100%;
			}

			.add-follow-btn {
				width: 40rpx;
				height: 40rpx;
				padding: 4rpx;
				border-radius: 100%;
				color: white;
				background-color: red;
				position: absolute;
				bottom: -24rpx;
				left: 50%;
				transform: translateX(-50%);
				font-weight: bold;
				text-align: center;
				line-height: 32rpx;

				.iconjia1 {
					font-size: 24rpx;
				}
			}
		}

		.icon-box {
			width: 100rpx;
			height: 100rpx;
			margin-top: 40rpx;
			display: flex;
			align-items: center;
			justify-content: space-around;
			flex-direction: column;
			color: white;

			.icon-btn {
				font-size: 60rpx;
			}

			.count-text {
				font-size: 24rpx;
			}
		}

	}

	.video-title {
		position: absolute;
		left: 20rpx;
		bottom: 20rpx;
		width: calc(100% - 160rpx);
		color: white;

		.user-name {
			font-weight: bold;
		}

		.video-content {
			font-size: 24rpx;
		}
	}
</style>
