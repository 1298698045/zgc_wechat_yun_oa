<template>
    <div class="wrap">
        <div class="header">
            <div class="row">
                <div class="avatar">
                    崔曼
                </div>
                <div class="info">
                    <p class="name">崔曼</p>
                    <p class="time">
                        信息中心    04-04  10:30
                    </p>
                </div>
            </div>
            <div class="title">
                太钢总医院医生对手术室护理工作满意调查表（2020.9）
            </div>
            <div class="describe">
                尊敬的各位医生：为充分体现医护一体化的开展，更好地提升科室护理工作的业务水平，促进医护间工作交流，特进行医护间满意度调查。请您对科内的护理工作给予真实、客观评价，在相应选项打“✓”，感谢您的配合。
            </div>
            <div class="box">
                <p class="time">
                    <i class="iconfont icon-shijian-copy"></i>
                    06月18日 周二 18:00 截止</p>
                <p class="num">
                    <i class="iconfont icon-canyuren"></i>
                    <span class="active">{{'10'}}</span>
                    <span>&nbsp;/&nbsp;{{'10'}}</span>
                </p>
            </div>
        </div>
        <div class="center">
            <div class="content" v-for="(item,index) in 6" :key="index">
                <p class="subject">1. 您所在院区</p>
                <div class="answer">
                    <p class="cont">烧伤</p>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return {

        }
    }
}
</script>
<style lang="scss">
@import '../../../../static/css/vote.css';
    .wrap{
        width: 100%;
        height: 100%;
        overflow: hidden;
        .header{
            padding: 0 26rpx;
            background: #fff;
            .row{
                display: flex;
                padding: 26rpx 0;
                .avatar{
                    width: 75rpx;
                    height: 75rpx;
                    line-height: 75rpx;
                    text-align: center;
                    font-size: 24rpx;
                    color: #fff;
                    background: #3399ff;
                    border-radius: 50%;
                }
                .info{
                    flex: 1;
                    margin-left: 25rpx;
                    .name{
                        font-size: 28rpx;
                        color: #333333;
                    }
                    .time{
                        font-size: 22rpx;
                        color: #999999;
                    }
                }
            }
            .title{
                font-size: 39rpx;
                color: #3399ff;
                font-weight: bold;
                padding: 21rpx 0;
            }
            .describe{
                font-size: 31rpx;
                color: #666666;
            }
            .box{
                display: flex;
                padding: 28rpx 0;
                align-items: center;
                margin-top: 28rpx;
                justify-content: space-between;
                p{
                    font-size: 24rpx;
                    color: #999999;
                    display: flex;
                    align-items: center;
                    i{
                        margin-right: 10rpx;
                    }
                }
                .num{
                    display: flex;
                    margin-left: 20rpx;
                    align-items: center;
                    span{
                        font-size: 24rpx;
                        color: #999999;
                    }
                    .active{
                        font-size: 35rpx;
                        color: #3399ff;
                        font-weight: bold;
                    }
                }
            }
        }
        .center{
            padding: 0 26rpx;
            background: #fff;
            .content{
                padding-bottom: 33rpx;
                .subject{
                    font-size: 33rpx;
                    color: #333333;
                    font-weight: bold;
                }
                .answer{
                    width: 100%;
                    background: #f8f8f8;
                    padding: 0 21rpx;
                    margin-top: 18rpx;
                    border-radius: 12rpx;
                    .cont{
                        font-size: 33rpx;
                        color: #333333;
                        padding: 37rpx 0;
                    }
                }
            }
        }
    }
</style>