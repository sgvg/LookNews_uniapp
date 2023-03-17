<template>
    <view class="deatil">
       <view class="title">
           {{deatil.title}}
       </view> 
       <view class="info">
           <view class="author">
               编辑：{{deatil.author}}
           </view>
           <view class="time">
              发布日期： {{timestampToTime(deatil.posttime)}}
           </view>
       </view>
       <view class="content">
           <rich-text :nodes="deatil.content"></rich-text>
       </view>
       <view class="description">
           鸡你太美鸡你太美鸡你太美鸡你太美鸡你太美鸡你太美鸡你太美鸡你太美鸡你太美鸡你太美鸡你太美
       </view>
    </view>
</template>

<script>
    export default {
        data() {
            return {
                deatil:{}
            };
        },
        onLoad(e) {
            this.getDeatil(e.cid,e.id)
        },
        methods:{
           getDeatil(cid,id){
               uni.request({
                   url:"https://ku.qingnian8.com/dataApi/news/detail.php",
                   data:{
                       cid,
                       id
                   },
                   success: (res) => {
                       console.log(res.data)
                       res.data.content = res.data.content.replace(/<img/gi,'<img style="max-width:100%"')
                       this.deatil = res.data
                       this.saveHistroy()
                       uni.setNavigationBarTitle({
                           title:this.deatil.title
                       })
                   }
               })
           },
           //格式化时间戳
           timestampToTime(timestamp) {
             // 时间戳为10位需*1000，时间戳为13位不需乘1000
             var date = new Date(timestamp * 1000);
             var Y = date.getFullYear() + "-";
             var M =
               (date.getMonth() + 1 < 10
                 ? "0" + (date.getMonth() + 1)
                 : date.getMonth() + 1) + "-";
             var D = (date.getDate() < 10 ? "0" + date.getDate() : date.getDate()) + " ";
             var h = date.getHours() + ":";
             var m = date.getMinutes() + ":";
             var s = date.getSeconds();
             return Y + M + D + h + m + s;
           },
           
           saveHistroy(){
               // let { id,classid,picurl} = this.deatil
               
               let historyArr = uni.getStorageSync('historyArr') || []
               
               
               
               let item = {
                   id:this.deatil.id,
                   classid:this.deatil.classid,
                   picurl:this.deatil.picurl,
                   title:this.deatil.title,
                   looktime:this.timestampToTime(new Date())  
               }
               
               let index = historyArr.findIndex(i=>{
                   return i.id == this.deatil.id
               }) 
               if(index>= 0){
                   historyArr.splice(index,1)
               }
               
               historyArr.unshift(item)
               uni.setStorageSync('historyArr',historyArr)
               
           }
        
            
        }
    }
</script>

<style lang="scss">
.deatil{
    padding: 30rpx;
    .title{
        font-size: 46rpx;
        color: #333;
    }
    .info{
        background-color: #F6F6F6;
        padding: 20rpx 10rpx;
        font-size: 22rpx;
        color: #666;
        display: flex;
        justify-content: space-between;
        margin: 40rpx 0;
    }
    .content{
        padding-bottom: 50rpx;
        
    }
    .description{
        background: #FEF0F0;
        font-size: 26rpx;
        padding: 20rpx;
        color: #F89898;
        line-height: 1.8em;
        
    }
}
</style>
