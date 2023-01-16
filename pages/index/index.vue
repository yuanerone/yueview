<template>
<view>
	<view class="swiper">
		<swiper class="swiper" :current="1" :circular="true" :vertical="true"
		@change="changefun" @animationfinish="animationfinishfun" >
			<swiper-item v-for="(item,index) in PayVideo">
				<view class="swiper-item">
					<video class="video" :id="'id'+index" loop="true" autoplay 
					:src="item.video_path" :custom-cache="false" :controls="false" 
					:enable-play-gesture="true" :enable-progress-gesture="true" 
					:show-center-play-btn="false" ></video>
				</view>
			</swiper-item>
		</swiper>
	</view>
	<view v-if="is_active">
		<view class="left">
			<cover-view class="left_box">
				<cover-view class="ren">@浙视频</cover-view>
				<cover-view class="ke_context">浙视频视频平台是浙江省第一网络原创视频生产基地,以浙江本地化新闻为中心,主打浙江省内突发新闻和热点资讯等视频内容,也是浙江省第一网络视频媒体平台。</cover-view>
			</cover-view>
		</view>
		<view class="right">
			<cover-view class="right_box">
				<cover-view class="top1">
					<cover-image class="avatar_img" src="http://git.ruan.work/uploads/-/system/user/avatar/12/avatar.png" mode=""></cover-image>
					<cover-image class="add_img" src="../../static/video/1.png" mode=""></cover-image>
				</cover-view>
				<cover-view class="top2">
					<cover-image class="t_img" src="../../static/video/2.png" mode=""></cover-image>
					<cover-view class="font_t">0</cover-view>
				</cover-view>
				<cover-view class="top2">
					<cover-image class="t_img" src="../../static/video/8.png" mode=""></cover-image>
					<cover-view class="font_t">0</cover-view>
				</cover-view>
				<cover-view class="top2">
					<cover-image class="t_img" src="../../static/video/3.png" mode=""></cover-image>
					<cover-view class="font_t">0</cover-view>
				</cover-view>
			</cover-view>
		</view>
	</view>
</view>
</template>

<script>
export default {
	data() {
		return {
			data: [
				{ "video_path": 'https://v-cdn.zjol.com.cn/280443.mp4' },
				{ "video_path": 'https://v-cdn.zjol.com.cn/276982.mp4' }, 
				{ "video_path": 'https://v-cdn.zjol.com.cn/276986.mp4' }, 
				{ "video_path": 'https://v-cdn.zjol.com.cn/276984.mp4' }, 
				{ "video_path": 'https://v-cdn.zjol.com.cn/276985.mp4' },
			],
			index_: 1,
			index: '1',
			is_active: true,
			active: 2,
			PayVideo: [],
			_arr: [],
			len: 0,
		}
	},
	methods: {
		changefun(e) {
			this.is_active = false
			let current = e.detail.current
			let { len, PayVideo, active,
				_arr, t, index_ } = this
			let videoContext = uni.createVideoContext('id' + index_)
			videoContext.pause()
			this.PayVideo = PayVideo
		},
		animationfinishfun(e) {
			let { index_, len, PayVideo,
				active, _arr, t } = this
			let current = e.detail.current
			this.is_active = true
			PayVideo[current]['istrue'] = true
			this.PayVideo = PayVideo
			let videoContext = uni.createVideoContext('id' + index_)
			videoContext.pause()
			videoContext = uni.createVideoContext('id' + current)
			videoContext.play()
			this.index_ = current
			if (PayVideo.length == len) {
				return
			}
			this.PayVideo.push(_arr[active])
			this.active += 1
		}
	},
	mounted() {
		uni.setStorageSync('video', this.data);
		let ind = 1
		this.index = ind
		const video = uni.getStorageSync('video');
		const len = video.length
		let b_arr = []
		let s_arr = []
		video.forEach((res, index) => {
			if (ind <= index) {
				b_arr.push(res)
			} else {
				s_arr.push(res)
			}
		})
		let _arr = b_arr.concat(s_arr.reverse())
		_arr = _arr.map(res => {
			res['istrue'] = false
			return res
		})
		_arr[0]['istrue'] = true
		const PayVideo = [
			_arr[len - 1], _arr[0], _arr[1]
		]
		this.PayVideo = PayVideo
		this._arr = _arr
		this.len = len
		this.$nextTick(function() {
			let videoContext = uni.createVideoContext('id1')
			videoContext.play()
		})
	}
}
</script>

<style scoped lang="less">
page{ font-family: PingFang SC; }
.swiper {
	height: 100vh;
	.swiper-item {
		position: relative;
		height: 100vh;
		background-color: #000000;
	}
}
.video {
	position: relative;
	width: 100%;
	height: 100vh;
}
.left_box {
	position: fixed;
	bottom: 60rpx;
	left: 30rpx;
	width: 516rpx;
	color: #FFFFFF;
	.ke {
		display: flex;
		align-items: center;
		cover-view { font-size: 26rpx; }
		cover-image {
			width: 40rpx;
			height: 40rpx;
			margin-right: 15rpx;
		}
	}
	.ren {
		font-size: 36rpx;
		font-weight: bold;
		margin: 20rpx 0;
	}
	.ke_context {
		font-size: 30rpx;
		word-break: break-all;
		word-wrap: break-word;
		white-space: pre-line;
		white-space: normal;
	}
	.auto {
		display: flex;
		align-items: center;
		margin-top: 24rpx;
		cover-view { font-size: 26rpx; }
		cover-image {
			width: 26rpx;
			height: 28rpx;
			margin-right: 10rpx;
		}
	}
}
.right_box {
	width: 100rpx;
	position: absolute;
	bottom: 60rpx;
	right: 12rpx;
	display: flex;
	flex-direction: column;
	color: #FFFFFF;
	.top1 {
		.avatar_img {
			width: 88rpx;
			height: 88rpx;
			border: 3px solid #FFFFFF;
			border-radius: 50%;
		}
		.add_img {
			width: 48rpx;
			height: 48rpx;
			margin: -20rpx auto 0;
		}
	}
	.top2 {
		text-align: center;
		margin-top: 37rpx;
		.t_img {
			height: 72rpx;
			width: 72rpx;
			margin: 0 auto 10rpx;
		}
		.font_t { font-size: 26rpx; }
	}
}
</style>
