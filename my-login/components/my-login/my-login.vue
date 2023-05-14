<template>
	<view >
	<view class="stopsx">
		<image src="@/static/stop.png" @click="stop_home"></image>
	</view>
	<view class="login-container">
	 <uni-icons type="contact-filled" size="30" color="#afafaf"></uni-icons>
	 <button type="primary" class="btn-login" open-type="getUserInfo" @getuserinfo="getUserinfo">本机号码一键登陆</button>
	<view class="xieyi">
		<label>
			<radio color="#c00000" />
			<text>已阅读并同意以下协议：</text>
			<text class="agree">《平台服务协议》 《隐私权政策》《注册相关协议》《账号认证服务协议》 </text>
			<text>，未注册的手机号将自动完成账号注册。</text>
		</label>
	</view>
	 <!-- <text class="tip-text">登录后尽享更多权益</text> -->
	 <text class="tip-text2">更多选项</text>
	</view>
	</view>
</template>
<script>
	import {mapMutations,mapState} from 'vuex'
	export default{
		name:'my-login',
		data(){
			return{
				
			};
		},
		computed:{
			...mapState('m_user',['redirectInfo'])
		},
		methods:{
			...mapMutations('m_user',['updateUserInfo','updateToken','updateRedirectInfo']),
			//用户授权后，获取用户的基本信息
			getUserinfo(e){
				console.log(e);
				
				
				if(e.detail.errMsg==='getUserInfo:fail auth deny') return uni.$showMsg('您取消了登陆')
				
				console.log(e.detail.userInfo);
				this.updateUserInfo(e.detail.userInfo)
				
				this.getToken(e.detail)
			},
			async getToken(info){
			const [err,res]= await uni.login().catch(err=>err)
			
			if(err || res.errMsg !=='login:ok') return uni.$showMsg('登陆失败')
			
			//准备参数
			const query={
				code: res.code,
									encryptedData: info.encryptedData,
									iv: info.iv,
									rawData: info.rawData,
									signature: info.signature
			}
			
			// const  {data: loginResult}= await uni.$http.post('/api/public/v1/users/wxlogin',query)
			// if(loginResult.meta.status !== 200) return uni.$showMsg('登录失败！')
			//直接把token保存到vuex中
			// this.updateToken(loginResult.message.token)
			
			const {data: loginResult} = await uni.$http.post('/api/public/v1/users/wxlogin',query)
							console.log(loginResult)
							this.updateToken('fade-token')	//在判断登录之前设置假token
							if(loginResult.meta.status !== 200) return uni.$showMsg("登录失败！")
							// this.navigateBack()
			},
			// navigateBack(){
			// 	if(this.redirectInfo && this.redirectInfo.openType==='switchTab')
			// }
			stop_home(){
				uni.switchTab({
				url:'/pages/home/home'
				})
			}
		}
		
		
	}
</script>
<style lang="scss">
	.stopsx image{
		width: 20px;
		height: 20px;
		margin: 10px;
	}
	.login-container{
		height: 750rpx;
		background-color: #f8f8f8;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		position: relative;
		overflow: hidden;
		
		.xieyi {
			width: 500rpx;
			font-size: 12px;
			
			
			.agree{
				color: #c00000;
			}
		}
		
		&::after{
			content: ' ';
			display: block;
			width: 100%;
			height: 40px;
			background-color: white;
			position: absolute;
			bottom: 0;
			left: 0;
			border-radius: 100%;
			transform: translateY(50%);
			
		}
		.btn-login{
			width: 90%;
			border-radius: 100px;
			margin: 15px 0;
			background-color: #c00000;
		}
		// .tip-text{
		// 	font-size: 12px;
		// 	color: gray;
		// }
		.tip-text2{
			margin-top: 25px;
			color: gray;
		}
	}
</style>
