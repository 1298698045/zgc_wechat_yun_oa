<template>
    <div class="wrap">
        <div class="contentWrap">
            <div class="rowContent" v-for="(item,index) in list" :key="index">
                <div class="rowT">
                    <div class="l">
                        <p>{{item.CreatedByName}}</p>
                    </div>
                    <div class="r">
                        <p>{{item.CreatedByName}}</p>
                        <p>{{item.BusinessUnitIdName}}</p>
                    </div>
                </div>
                <p class="title">
                    {{item.CreatedByName}}
                </p>
                <div class="cont">
                    {{item.cont}}
                </div>
                <div class="more">
                    <p>
                        {{item.CreatedOn}}
                    </p>
                    <p @click="getMore" v-if="isEdit">
                        <i-icon type="more" color="#666666" size="20" />
                    </p>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return {
            list:[
                {
                    CreatedByName:'测试',
                    BusinessUnitIdName:'部门',
                    cont:"123"
                }
            ]
        }
    }
}
</script>
<style lang="scss">
    .wrap{
        width: 100%;
        height: 100%;
        overflow: hidden;
        .contentWrap{
            padding: 0 24rpx; 
            .rowContent{
                background: #fff;
                border-radius: 23rpx;
                margin: 24rpx 0;
                padding: 33rpx;
                .rowT{
                    display: flex;
                    align-items: center;
                    .l{
                        p{
                            width: 72rpx;
                            height: 72rpx;
                            line-height: 72rpx;
                            text-align: center;
                            background: #3399ff;
                            color: #fff;
                            font-size: 26rpx;
                            border-radius: 50%;
                        }
                    }
                    .r{
                        flex: 1;
                        margin-left: 25rpx;
                        p:nth-child(1){
                            font-size: 32rpx;
                            color: #333333;
                        }
                        p:nth-child(2){
                            font-size: 26rpx;
                            color: #999999;
                        }
                    }
                }
                .title{
                    font-size: 34rpx;
                    color: #333333;
                    font-weight: bold;
                    margin: 30rpx 0;
                }
                .cont{
                    font-size: 34rpx;
                    color: #333333;
                    // margin-bottom: 50rpx;
                }
                .files{
                    padding: 19rpx;
                    border: 1rpx solid #e7e8ea;
                    display: flex;
                    align-items: center;
                    margin: 10rpx 0;
                    .l{
                        width: 108rpx;
                        height: 108rpx;
                        img{
                            width: 100%;
                            height: 100%;
                            overflow: hidden;
                        }
                    }
                    .r{
                        margin-left: 25rpx;
                        p:nth-child(1){
                            width: 300rpx;
                            font-size: 28rpx;
                            color: #333333;
                            overflow: hidden;
                            text-overflow:ellipsis;
                            white-space: nowrap;
                            padding-bottom: 20rpx;
                        }
                        p:nth-child(2){
                            font-size: 24rpx;
                            color: #7e8184;
                        }
                    }
                }
                .more{
                    display: flex;
                    justify-content: space-between;
                    p{
                        font-size: 24rpx;
                        color: #a1a5aa;
                    }
                }
            }
        }
        .footer{
            width: 100%;
            position: fixed;
            bottom: 20rpx;
            background: #fff;
            p{
                text-align: center;
                font-size: 34rpx;
                color: #3399ff;
                padding: 30rpx 0;
            }
        }
    }
</style>