<template>
	<view class="home">
        <view class="scrollNav">
            <scroll-view scroll-x class="navscroll">
                <view class="item" :class="index==navindex ? 'active' : ''" v-for="(item,index) in navArr" @click="clicnav(index,item.id)" :key="item.id">{{item.classname}}</view>
                
            </scroll-view>
        </view>
        <view class="content">
            <view class="row" v-for="item in newsArr":key="item.id">
                <newsbox :item="item" @click.native="goDeatil(item)"></newsbox>
            </view>
            
        </view>
        <view class="loading">
            <view class="">
                
            </view>
            <view class="" v-show="loading == 1">
                数据加载中。。。。。。。
            </view>
            <view class="" v-show="loading == 2">
                没有更多了。。。。。。。
            </view>
        </view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navindex:0,
                navArr:[],
                newsArr:[],
                currentPage:1,
                Cid:50,
                
                loading:0   //0默认 1加载中 2没有更多了
			}
		},
		onLoad() {
           this.getNav()
           this.getNews()
		},
        onReachBottom() {
            if(this.loading == 2){
                return
            }
            this.currentPage++
            this.loading = 1
            this.getNews(this.Cid)
        },
		methods: {
            //切换导航栏显示高光
            clicnav(index,id){
                this.navindex = index
                this.currentPage = 1
                this.newsArr = []
                this.Cid = id
                this.loading = 0
                
                this.getNews(id)
            },
            //跳转详情页
            goDeatil(item){
                uni.navigateTo({
                    url:`../deatil/deatil?cid=${item.classid}&id=${item.id}`
                })
            },
            
            //获取导航栏
            getNav(){
                uni.request({
                    url:"https://ku.qingnian8.com/dataApi/news/navlist.php",
                    success: (res) => {
                        this.navArr = res.data
                        
                    }
                })
            },
            //获取新闻列表
            getNews(id=50){
                uni.request({
                    url:"https://ku.qingnian8.com/dataApi/news/newslist.php",
                    data:{
                        
                        cid:id,
                        page:this.currentPage
                    },
                    success: (res) => {
                        
                        if(res.data.length == 0){
                            this.loading = 2
                        }
                        this.newsArr = [...this.newsArr,...res.data]
                        console.log(res.data)
                    }
                })
            }
		}
	}
</script>

<style lang="scss" scoped>
.navscroll{
    height: 100rpx;
    background-color:#F7F8FA;
    white-space: nowrap;
    position: fixed;
    top: var(--window-top);
    left: 0;
    z-index: 10;
    
    
    /deep/ ::-webkit-scrollbar{
        width: 4px !important;
        height: 1px !important;
        overflow: auto !important;
        background: transparent !important;
        -webkit-appearance: aouto !important;
        display: block;
    }
    .item{
        font-size: 40rpx;
        display: inline-block;
        line-height: 100rpx;
        padding: 0 30rpx;
        color: #333;
        &.active{
            color: #31c27c;
        }
    }
}
.content{
    padding: 30rpx;
    padding-top: 130rpx;
    .row{
        border-bottom: 1px dashed #efefef;
        padding: 20rpx 0;
    }
}
.loading{
    text-align: center;
    font-size: 20rpx;
    color: #888;
}
</style>
