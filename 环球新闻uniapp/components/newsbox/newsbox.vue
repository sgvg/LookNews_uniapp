<template>
    <view class="newsbox">
        <view class="pic">
            <image :src="item.picurl" mode="aspectFill"></image>
        </view>
        <view class="text">
            <view class="title">
                {{item.title}}
            </view>
            <view class="info" v-if="!item.looktime">
                <text>{{item.author}}</text>
                <text>{{item.hits}}浏览</text>
            </view>
            <view class="info" v-else>
                <text>浏览时间{{item.looktime}}</text>
            </view>
        </view>
    </view>
</template>

<script>
    export default {
        name:"newsbox",
        props:{
            item:{
                type:Object,
                default(){
                    return{
                        title:"组件内默认标题",
                        author:"蔡徐坤",
                        hits:6669,
                        picurl:"",
                        looktime:"2023-3-16 23:34:56"
                    }
                }
            }
        },
        data() {
            return {
                
            };
        }
    }
</script>

<style lang="scss">
.newsbox{
    display: flex;
    .pic{
        width: 230rpx;
        height: 160rpx;
        image{
            width: 100%;
            height: 100%;
        }
    }
    .text{
        // border: 1px red solid;
        flex: 1;
        padding-left: 20rpx;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        .title{
            font-size: 38rpx;
            color: #333;
            text-overflow: -o-ellipsis-lastline;
            overflow: hidden ;   
            text-overflow: ellipsis;
            display: -webkit-box;
            -webkit-line-clamp:2;
            line-clamp:2;
            -webkit-box-orient:vertical;
        }
        .info{
            font-size: 26rpx;
            color: #999;
        }
    }
}
</style>
