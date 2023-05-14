<template>
	<view>
		<view class="goods-list">
			<view v-for="(goods,i) in goodsList" :key="i" @click="gotoDatail(goods)">
			<my-goods :goods="goods"></my-goods>
				
			</view>
			
		</view>
	</view>
</template>

<script>

	export default {
		data() {
			return {
				queryObj:{
					query:'',
					cid:'',
					pagenum:1,
					pagesize:10
				},
				goodsList:[],
				total:0,
				//节流阀
				isloding:false
				
			};
		},
		onLoad(options) {
			this.queryObj.query=options.query || ''
			this.queryObj.cid=options.cid || ''
			
			this.getGoodsList()
		},
		methods:{
			//获取商品列表数据的方法
			async getGoodsList(cb){
				//打开节流阀
				this.isloding=true
			 const {data:res} = await uni.$http.get('/api/public/v1/goods/search',this.queryObj)
			 //关闭节流阀
			 this.isloding=false
			 cb && cb()
			 if(res.meta.status !==200) return uni.$showMsg()
			
			 
			 this.goodsList = [...this.goodsList,...res.message.goods]
			 this.total=res.message.total
			
	
			}, 
			gotoDatail(goods){
				uni.navigateTo({
					url:'/subpkg/goods_detail/goods_detail?goods_id' + goods.goods_id
				})
			}
		},
		onReachBottom(){
			if(this.queryObj.pagenum* this.queryObj.pagesize>=this.total)return uni.$showMsg('数据加载完毕')
			if(this.isloding)return
			//让页码值自增加1
			this.queryObj.pagenum++
			this.getGoodsList() 
		},
		onPullDownRefresh(){
			//重置关键数据
			this.queryObj.pagenum=1
			this.total=0
			this.isloding=false
			this.goodsList=[]
			
			this.getGoodsList(()=>uni.stopPullDownRefresh())
		}
	}
</script>

<style lang="scss">

</style>
