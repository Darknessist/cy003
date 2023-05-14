<template>
	<view class="my-userinfo-container">
		<!-- 头像，昵称区 -->
		<view class="top-box">
			<!-- <image :src="userinfo.avatarUrl" class="avatar"></image> -->
			<image src="@/static/logo.png" class="avatar"></image>
			<view class="nickname">{{userinfo.nickName}}</view>
			
		</view>
		<!-- 面板区域 -->
		<view class="panel-list">
			<!-- 第一个面板 -->
			<view class="panel">
				<view class="panel-body"  @click="turndz">
					<view class="panel-item">
						<text>8</text>
						<text>收藏的店铺</text>
					</view>
					<view class="panel-item">
						<text>14</text>
						<text>收藏的商品</text>
					</view>
					<view class="panel-item">
						<text>18</text>
						<text>关注的商品</text>
					</view>
					<view class="panel-item">
						<text>84</text>
						<text>足迹</text>
					</view>
				</view>
			</view>
			
			<!-- 第二个面板区域 -->
			<view class="panel">
				<view class="panel-title">
					我的订单
					<view class="all">
						全部>
					</view>
					</view>
					<view class="panel-body" @click="turndz">
						<view class="panel-item">
							<image src="/static/fukuan.png" class="icon"></image>
							<text>待付款</text>
						</view>
						<view class="panel-item">
							<image src="/static/fahuo.png" class="icon"></image>
							<text>待发货</text>
						</view>
						<view class="panel-item">
							<image src="/static/shouhuo.png" class="icon"></image>
							<text>待收货</text>
						</view>
						<view class="panel-item">
							<image src="/static/pj.png" class="icon"></image>
							<text>待评价</text>
						</view>
						<view class="panel-item">
							<image src="/static/tk.png" class="icon"></image>
							<text>退款/售后</text>
						</view>
					</view>
					<view class="goods_qs">
						<view class="goods_icon">
							<image src="@/static/logo.png" ></image>
						</view>
						<view class="goods_contain">
						<view class="goods_top">
							<view class="goods_left">已签收</view>
							<view class="goods_right">04-24</view>
						</view>
						<view class="goods_qj">
							四川传媒学院影视硅谷菜鸟驿站完成取件...
						</view>
						</view>
					</view>
				
					<view class="kongbai">
						
					</view>
				
			</view>
			<!-- 第三个 -->
			<view class="panel">
				<view class="panel-title">
					我的权益
					<view class="all">
						全部>
					</view>
					</view>
				<view class="panel-body"  @click="turndz">
					<view class="panel-item">
						<text>￥0.00</text>
						<text>红包</text>
					</view>
					<view class="panel-item">
						<text>4张</text>
						<text>优惠劵</text>
					</view>
					<view class="panel-item">
						<text>开卡享￥64</text>
						<text>省钱卡</text>
					</view>
					<view class="panel-item">
						<text>￥1.05</text>
						<text>可抵</text>
					</view>
				</view>
			</view>
			<!-- 第四个 -->
			<view class="panel">
				<view class="panel-list-item" @click="turndz">
					<text>收获地址</text>
					<uni-icons type="arrowright" size="15" ></uni-icons>
				</view>
				<view class="panel-list-item" @click="contact">
					<text>联系客服</text>
					<uni-icons type="arrowright" size="15"></uni-icons>
				</view>
				<view class="panel-list-item" @click="logout">
					<text>退出登录</text>
					<uni-icons type="arrowright" size="15"></uni-icons>
				</view>
				
			</view>
			
		</view>
	
	</view>
</template>
<script>
	import {mapState ,mapMutations} from 'vuex'
	
	export default{
		name:'my-userinfo',
		data(){
			return{
				
			};
		},
		computed:{
			...mapState('m_user',['userinfo']),
		},
		methods:{
			...mapMutations('m-user',['updateAddress','userUserInfo','updateToken']),
			  async logout(){
				const[err,succ]=await uni.showModal({
					title:'提示',
					content:'确认退出吗',
				}).catch(err=>err)
				if(succ &succ.confirm){
					this.updateAddress({})
					this.userUserInfo({})
					this.updateToken('')
				}
			},
			turndz(){
				uni.switchTab({
					url:'/pages/cart/cart'
				})
			},
			contact(){
				uni.switchTab({
					url:'/pages/home/home'
				})
			}
		}
	}
</script>
<style lang="scss">
	.my-userinfo-container{
		height: 100%;
		background-color: #f4f4f4;
		.top-box{
			height: 400rpx;
			background-color: #c00000;
			display: flex;
			justify-content: center;
			align-items: center;
			flex-direction: column;
			.avatar{
				width: 90px;
				height: 90px;
				border-radius: 45px;
				border: 2px solid #fff;
				box-shadow: 0 1px 5px black;
				
			}
			.nickname{
				font-size: 16px;
				color: #fff;
				font-weight: bold;
				margin-top: 10px;
			}
		}
	}
	.panel-list{
		padding: 0 10px;
		position: relative;
		top: -10px;
		.panel{
			background-color: white;
			border-radius: 3px;
			margin-bottom: 8px;
			.panel-title{
				display:flex;
				justify-content: space-between;
				line-height: 45px;
				padding-left: 10px;
				font-size: 15px;
				border-bottom: 1px solid #f4f4f4;
				.all{
					font-size:12px;
					color:#c3c3c3;
				}
				
			}
			.goods_qs{
				background-color: #f0f0f0;
				display:flex;
				border-radius:5px; 
				margin:10px 20px ;
				.goods_icon image{
					height: 35px;
					width: 35px;
				}
				.goods_top{
					display:flex;
					justify-content: space-between;
					font-size:13px;
					.goods_right{
						color:#c3c3c3;
					}
				}
				.goods_qj{
					color:#c3c3c3;
					font-size:13px;
				}
				.goods_bottom{
			        display: inline;
				}
			}
			.kongbai{
				height: 5px;
				background-color:white;
			}
			.panel-body{
				display: flex;
				justify-content: space-around;
				.panel-item{
					display: flex;
					flex-direction: column;
					align-items: center;
					justify-content: space-around;
					padding: 10px 0;
					font-size: 14px;
					.icon{
						width: 20px;
						height: 20px;	
					}
				}
			}
		}
	}
	.panel-list-item{
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-size: 15px;
		padding:0 10px;
		line-height: 45px;
	}
</style>
