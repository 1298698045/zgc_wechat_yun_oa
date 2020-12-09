<template>
    <div class="wrap">
        <div class="header">
            <p @click="getReturn">取消</p>
            <p @click="getSend">发送</p>
        </div>
        <div class="center">
            <div class="content">
                <textarea name="" id="" cols="30" rows="10" maxlength="100000"
                 :auto-height="true"
                 :hold-keyboard="keyboard"
                 @focus="getFocus"
                 @blur="getBlur"
                 v-model="description"
                  placeholder="分享新动态..."></textarea>
            </div>
        </div>
        <div class="footer" @click="getKeyboard" :style="{'bottom':keyboardHeight+'px'}" :class="{'active':isModelmes}" catchtouchmove="true">
            <div class="active_box" :class="{'active':isModelmes}">
                <p class="position" @click="getLocation">你在哪里?</p>
                <p class="open">公开</p>
            </div>
            <div class="box">
                <p @click="getPhotograph"></p>
                <p @click="getUploadImg"></p>
                <p></p>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return {
            isFocus:false,
            keyboardHeight:"",
            keyboard:false
        }
    },
    computed:{
        isModelmes(){
            return wx.getStorageSync('isModelmes');
        },
        sessionkey(){
            return wx.getStorageSync('sessionkey');
        }
    },
    onLoad(){
        wx.onKeyboardHeightChange(res => { //监听键盘高度变化
            console.log(res.height,'res');
            this.keyboardHeight = res.height;
        })
    },
    methods:{
        getKeyboard(){
            this.keyboard = true;
        },
        getFocus(e){
            this.isFocus = true;
            
        },
        getBlur(e){
            this.isFocus = false;
        },
        getReturn(){
            wx.navigateBack({
                delta:1
            })
        },
        getLocation(){
            wx.getLocation({
                type: 'gcj02', //返回可以用于wx.openLocation的经纬度
                success (res) {
                    const latitude = res.latitude
                    const longitude = res.longitude
                    wx.openLocation({
                        latitude,
                        longitude,
                        scale: 18
                    })
                }
            })
        },
        getPhotograph(){
            wx.chooseImage({
                count: 1,
                sizeType: ['original', 'compressed'],
                sourceType: ['camera'],
                success (res) {
                    // tempFilePath可以作为img标签的src属性显示图片
                    const tempFilePaths = res.tempFilePaths
                }
            })
        },
        getUploadImg(){
            wx.chooseImage({
                count: 1,
                sizeType: ['original', 'compressed'],
                sourceType: ['album', 'camera'],
                success (res) {
                    // tempFilePath可以作为img标签的src属性显示图片
                    const tempFilePaths = res.tempFilePaths
                }
            })
        },
        getSend(){
            this.$httpWX.get({
                url:this.$api.message.queryList,
                data:{
                    method:this.$api.community.send,
                    SessionKey:this.sessionkey,
                    status:this.description,
                    visible:1
                }
            }).then(res=>{
                if(res.status==1){
                    wx.showToast({
                        title:res.msg,
                        icon:"none",
                        duration:2000
                    })
                }
            })
        }
    }
}
</script>
<style lang="scss">
    .wrap{
        width: 100%;
        height: 100%;
        overflow: hidden;
        .header{
            width: 100%;
            position: fixed;
            top: 0;
            display: flex;
            justify-content: space-between;
            padding: 30rpx;
            color: #3399ff;
        }
        .center{
            .content{
                margin-top: 100rpx;
                background: #fff;
                height: 91vh;
                padding: 40rpx 33rpx;
                textarea{
                    width: 100%;
                    height: 100% !important;
                    font-size: 33rpx;
                    // background: red;
                }
            }
        }
        .footer.active{
            padding-bottom: 50rpx;
        }
        .footer{
            width: 100%;
            position: fixed;
            bottom: 0;
            background: #fff;
            border-top: 1rpx solid #e2e3e5;
            z-index: 99999;
            .active_box.active{
                bottom: 180rpx !important;
            }
            .active_box{
                width: 100%;
                position: absolute;
                bottom: 130rpx;
                z-index: 99999;
                display: flex;
                justify-content: space-between;
                padding: 0 30rpx;
                .position{
                    width: 194rpx;
                    height: 56rpx;
                    line-height: 56rpx;
                    border-radius: 28rpx;
                    font-size: 28rpx;
                    color: #505050;
                    text-align: center;
                    background: #f8f8f8;
                    border: 1rpx solid #cccccc;
                }
                .open{
                    width: 125rpx;
                    height: 56rpx;
                    line-height: 56rpx;
                    border-radius: 28rpx;
                    font-size: 28rpx;
                    color: #505050;
                    text-align: center;
                    background: #f8f8f8;
                    border: 1rpx solid #cccccc;
                }
            }
            .box{
                display: flex;
                padding: 26rpx 33rpx;
                p{
                    width: 49rpx;
                    height: 49rpx;
                    background: #999999;
                    margin-right: 67rpx;
                }
            }
        }
    }
</style>