<template>
    <div class="wrap">
        <div class="center">
            <div class="content">
                <div class="row">
                    <div class="avatar">
                        <p>崔曼</p>
                    </div>
                    <div class="rBox">
                        <div class="top">
                            <p class="name">崔曼</p>
                        </div>
                        <div class="info">
                            信息中心    04-04  10:30
                        </div>
                    </div>
                </div>
                <p class="text">1.医院召开全体人员培训；
                    2.学习规章制度；
                    3.财务流程；
                </p>
            </div>
            <div class="container">
                <div class="comment_wrap">
                    <div class="row_box">
                        <div class="tabs">
                            <p :class="{'active':idx==index}" v-for="(item,index) in tabs" :key="index" @click="getTab(item,index)">{{item}}</p>
                        </div>
                        <div class="zan">
                            <p>赞 401</p>
                        </div>
                    </div>
                    <div class="comment">
                        <div class="box" v-for="(item,index) in list" :key="index">
                            <div class="avatar">{{item.CreatedByName}}</div>
                            <div class="left_cont">
                                <div class="name">
                                    <p>{{item.CreatedByName}}</p>
                                    <p class="num" @click="getLikeItem(item)">{{item.LikeQty}}</p>
                                </div>
                                <p class="text">{{item.Comment}}</p>
                                <p class="info">信息中心    {{item.CreatedOn}}</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="footer" :class="{'bottomActive':isModelmes,'footImt':!isModelmes}">
            <div class="box">
                <p>转发</p>
                <p @click="getComment">评论</p>
                <p>赞</p>
            </div>
        </div>
        <div class="keyboard" v-if="isFocus" :style="{'bottom':keyboardHeight+'px'}" catchtouchmove="true">
            <div class="inp_box">
                <p>
                    <input type="text" v-model="comment" :hold-keyboard="keyboard" :auto-focus="true" :adjust-position="false" @blur="getBlur">
                </p>
                <p @click.stop="getSubmit">发送</p>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return {
            idx:0,
            tabs:['转发 321','评论 56'],
            keyboardHeight:"",
            isFocus:false,
            id:"",
            PageSize:20,
            pageNumber:1,
            keyboard:true,
            list:[]
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
    onLoad(options){
        this.id = options.id;
        wx.onKeyboardHeightChange(res => { //监听键盘高度变化
            console.log(res.height,'res');
            this.keyboardHeight = res.height;
        })
        this.queryComment();
    },
    methods:{
        queryComment(){
            this.$httpWX.get({
                url:this.$api.message.queryList,
                data:{
                    method:this.$api.community.queryComment,
                    SessionKey:this.sessionkey,
                    scope:"Comment",
                    PageSize:this.PageSize,
                    pageNumber:this.pageNumber
                }
            }).then(res=>{
                console.log(res);
                this.list = res.listData;
            })
        },
        getTab(item,index){
            this.idx = index;
        },
        getComment(){
            this.isFocus = true;
        },
        getBlur(e){
            this.isFocus = false;
        },
        getFocus(){
            this.isFocus = true;
        },
        getSubmit(){
            this.keyboard = true;
            this.$httpWX.get({
                url:this.$api.message.queryList,
                data:{
                    method:this.$api.community.comment,
                    SessionKey:this.sessionkey,
                    objectid:this.id,
                    comments:this.comment,
                    objTypeCode:6000
                }
            }).then(res=>{
                console.log(res);
                this.queryComment();
                this.isFocus = false;
            })
        },
        getLikeItem(item){
            this.$httpWX.get({
                url:this.$api.message.queryList,
                data:{
                    method:this.$api.community.like,
                    SessionKey:this.sessionkey,
                    objectid:6000,
                    id:item.ChatterId,
                    action:'like'
                }
            }).then(res=>{
                console.log(res);
                this.queryComment();
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
        .content{
            padding: 26rpx 33rpx;
            background: #fff;
            margin-top: 33rpx;
            .row{
                display: flex;
                align-items: center;
                .avatar{
                    width: 75rpx;
                    height: 75rpx;
                    line-height: 75rpx;
                    text-align: center;
                    border-radius: 50%;
                    background: #3399ff;
                    font-size: 21rpx;
                    color: #fff;
                }
                .rBox{
                    flex: 1;
                    margin-left: 20rpx;
                    .top{
                        display: flex;
                        justify-content: space-between;
                        .name{
                            font-size: 28rpx;
                            color:#333333;
                        }
                    }
                    .info{
                        font-size: 22rpx;
                        color: #999999;
                    }
                }
            }
            .text{
                padding: 28rpx 0;
                font-size: 28rpx;
                color: #333333;
            }
        }
        .container{
            padding-bottom: 150rpx;
            .comment_wrap{
                margin-top: 17rpx;
                background: #fff;
                .row_box{
                    display: flex;
                    justify-content: space-between;
                    align-items: center;
                    border-bottom: 1rpx solid #e2e3e5;
                    padding: 0 38rpx;
                    font-size: 29rpx;
                    color: #333333;
                    font-weight: bold;
                    .tabs{
                        display: flex;
                        p{
                            width: 146rpx;
                            text-align: center;
                            padding: 30rpx 0;
                        }
                        p.active{
                            border-bottom: 4rpx solid #3399ff;
                        }
                    }
                }
                .comment{
                    padding: 0 38rpx;
                    .box{
                        display: flex;
                        padding: 26rpx 0;
                        .avatar{
                            width: 75rpx;
                            height: 75rpx;
                            line-height: 75rpx;
                            text-align: center;
                            border-radius: 50%;
                            background: #3399ff;
                            font-size: 21rpx;
                            color: #ffffff;
                        }
                        .left_cont{
                            flex: 1;
                            margin-left: 20rpx;
                            .name{
                                font-size: 28rpx;
                                color: #3399ff;
                                display: flex;
                                justify-content: space-between;
                                .num{
                                    color: #999999;
                                    font-size:25rpx;
                                }
                            }
                            .text{
                                font-size: 32rpx;
                                color: #333333;
                                padding: 10rpx 0;
                            }
                            .info{
                                font-size: 22rpx;
                                color: #644d4d;
                            }
                        }
                    }
                }
            }
        }
        .footer{
            width: 100%;
            position: fixed;
            bottom: 0;
            background: #fff;
            .box{
                display: flex;
                padding: 24rpx 0;
                p{
                    flex: 1;
                    text-align: center;
                    font-size: 28rpx;
                }
            }
        }
        .keyboard{
            width: 100%;
            position: fixed;
            bottom: 0;
            background: #fff;
            .inp_box{
                display: flex;
                padding: 19rpx 25rpx;
                align-items: center;
                p:nth-child(1){
                    flex: 1;
                    input{
                        width: 100%;
                        height: 67rpx;
                        background: #f3f5f4;
                        font-size: 28rpx;
                    }
                }
                p:nth-child(2){
                    width: 100rpx;
                    font-size: 28rpx;
                    color: #3399ff;
                    text-align: center;
                }
            }
        }
    }
</style>