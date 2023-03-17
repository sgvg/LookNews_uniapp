<template>
    <view class="user">
        <view class="top">
            <image src="../../static/logo.png" mode=""></image>
            <view class="text">
                浏览历史
            </view>
        </view>
        <view class="content">
            <view class="row" v-for="item in DataArr" :key="item.id">
                <newsbox :item="item" @click.native="goDeatil(item)"></newsbox>
            </view>
        </view>
    </view>
</template>

<script>
    export default {
        data() {
            return {
                DataArr:[]
            };
        },
        /* onLoad() {
            this.getData()
        }, */
        onShow() {
            this.getData()
        },
        methods:{
            //跳转详情页
            goDeatil(item){
                uni.navigateTo({
                    url:`../deatil/deatil?cid=${item.classid}&id=${item.id}`
                })
            },
            getData(){
                this.DataArr = uni.getStorageSync('historyArr') || []
            }
        }
    }
</script>

<style lang="scss">
.user{
    .top{
        padding: 50rpx 0;
        background-color: #F8F8F8;
        color: #555;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        image{
            width: 150rpx;
            height: 150rpx;
        }
        .text{
            font-size: 38rpx;
            padding-top: 20rpx;
        }
    }
    .content{
        padding: 30rpx;
        .row{
            border-bottom: 1px dashed #efefef;
            padding: 20rpx 0;
        }
    }
}
</style>
