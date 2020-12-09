<template>
    <div class="wrap">
        <div class="center">
            <div class="content" v-for="(item,index) in list" :key="index" @click="getDetail(item)">
                <div class="row">
                    <div class="avatar">
                        <p>{{item.OwningUserName}}</p>
                    </div>
                    <div class="rBox">
                        <div class="top">
                            <p class="name">{{item.OwningUserName}}</p>
                            <p @click.stop="getMore(item)"><i-icon type="more" color="#999999" size="20" /></p>
                        </div>
                        <div class="info">
                            信息中心    {{item.CreatedOn}}
                        </div>
                    </div>
                </div>
                <p class="text">
                    {{item.Description}}
                </p>
                <div class="more_btn">
                    <div class="btn">转发</div>
                    <div class="btn">评论</div>
                    <div class="btn" @click.stop="getLike(item)">
                        赞
                        <span>{{item.NumOfLike}}</span>
                    </div>
                </div>
            </div>
        </div>
        <div class="footer" :class="{'bottomActive':isModelmes,'footImt':!isModelmes}">
            <div class="box_wrap">
                <div class="box" @click="getWriteJournal">
                    <p></p>
                    <p>写日志</p>
                </div>
                <div class="box">
                    <p></p>
                    <p>看日志</p>
                </div>
                <div class="box">
                    <p></p>
                    <p>统计</p>
                </div>
                <div class="box">
                    <p></p>
                    <p>设置</p>
                </div>
            </div>
        </div>
        <van-action-sheet
            :show="show"
            :round="false"
            cancel-text="取消"
            @close="onClose"
            @cancel="onClose"
        >
            <div class="sheetWrap">
                <p>收藏</p>
                <p @click="getDelete">删除</p>
            </div>
        </van-action-sheet>
    </div>
</template>
<script>
export default {
    data(){
        return {
            pageNumber:1,
            pageSize:25,
            list:[],
            show:false,
            id:""
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
        this.getQuery();
    },
    methods:{
        getQuery(){
            this.$httpWX.get({
                url:this.$api.message.queryList,
                data:{
                    method:this.$api.community.query,
                    SessionKey:this.sessionkey,
                    pageNumber:this.pageNumber,
                    pageSize:this.pageSize
                }
            }).then(res=>{
                this.list = res.listData;
            })
        },
        getLike(item){
            this.$httpWX.get({
                url:this.$api.message.queryList,
                data:{
                    method:this.$api.community.like,
                    SessionKey:this.sessionkey,
                    id:item.ChatterId,
                    ObjTypeCode:6000,
                    action:'like'
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
        },
        getMore(item){
            this.id = item.ChatterId;
            this.show = true;
        },
        onClose(){
            this.show = false;
        },
        getDelete(){
            this.$httpWX.get({
                url:this.$api.message.queryList,
                data:{
                    method:this.$api.community.del,
                    SessionKey:this.sessionkey,
                    Id:this.id
                }
            }).then(res=>{
                console.log(res);
                if(res.status==1){
                    wx.showToast({
                        title:res.msg,
                        icon:"success",
                        duration:2000
                    })
                }
                this.show = false;
                this.getQuery();
            })
        },
        getDetail(item){
            const url = '/pages/community/detail/main?id='+item.ChatterId;
            wx.navigateTo({url:url});
        },
        getWriteJournal(){
            const url = '/pages/community/sendDynamic/main';
            wx.navigateTo({url:url});
        }
    },
        // 下拉刷新
    onPullDownRefresh() {
        this.getQuery();
        wx.stopPullDownRefresh();
    },
    /**
     * 页面上拉触底事件的处理函数
     */
    onReachBottom() {
    }
}
</script>
<style lang="scss">
    .wrap{
        width: 100%;
        height: 100%;
        overflow: hidden;
        .content{
            padding: 0 33rpx;
            background: #fff;
            margin-top: 33rpx;
            .row{
                display: flex;
                align-items: center;
                padding-top: 26rpx;
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
                border-bottom: 1rpx solid #e2e3e5;
            }
            .more_btn{
                font-size: 28rpx;
                color: #526992;
                padding: 24rpx 0;
                display: flex;
                justify-content: space-around;
            }
        }
        .footer{
            width: 100%;
            position: fixed;
            bottom: 0;
            background: #fff;
            .box_wrap{
                display: flex;
                padding: 24rpx 0;
                .box{
                    flex: 1;
                    text-align: center;
                    font-size: 22rpx;
                    color: #a1a5a6;
                }
            }
        }
        .sheetWrap{
            background: #fff;
            p{
                text-align: center;
                font-size: 28rpx;
                padding: 30rpx 0;
                border-bottom: 1rpx solid #e2e3e5;
                color: #3399ff;
            }
        }
    }
</style>