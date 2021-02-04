<template>
    <div class="wrap">
        <div class="header">
            <div class="nav">
                <div class="search">
                    <input type="text" placeholder="搜索" placeholder-class="placeholder" />
                </div>
                <div class="tabs">
                    <i-tabs :current="current" scroll @change="handleChangeScroll">
                        <i-tab key="tab1" title="全部"></i-tab>
                        <i-tab key="tab2" title="院办"></i-tab>
                        <i-tab key="tab3" title="临床科"></i-tab>
                        <i-tab key="tab4" title="医技科"></i-tab>
                        <i-tab key="tab5" title="选项5"></i-tab>
                        <i-tab key="tab6" title="选项6"></i-tab>
                        <i-tab key="tab7" title="选项7"></i-tab>
                        <i-tab key="tab8" title="选项8"></i-tab>
                        <i-tab key="tab9" title="选项9"></i-tab>
                    </i-tabs>
                </div>
                <div class="more">

                </div>
            </div>
            <div class="screen">
                <div class="select">
                    <van-dropdown-menu>
                        <!-- <van-dropdown-item id="item" title="所有编制">
                            <div class="list">
                                <p class="name" @change="changeDropDown" :class="{'active':num==index}" v-for="(item,index) in listMenu" :key="index" @click="getCheck(item,index)">{{item}}</p>
                            </div>
                        </van-dropdown-item> -->
                        <van-dropdown-item :value=" value1 " :options=" listMenu " />
                    </van-dropdown-menu>
                </div>
                <div class="right">
                    <div class="box" @click="getSwitch"></div>
                    <div class="box"></div>
                </div>
            </div>
        </div>
        <div class="center" v-if="isBook==false">
            <div class="content">
                <div class="box_card" v-for="item in list">
                    <div class="imgs">

                    </div>
                    <div class="Copywriting">
                        <p class="name">
                            <span>卫辉</span>
                            <span>在编</span>
                        </p>
                        <p class="depart">
                            临床科 · 主治医师
                        </p>
                        <div class="desc">
                            血液疾病、多发性骨髓瘤、干细胞移植。
                        </div>
                    </div>
                </div>
            </div>
            
        </div>
        <div class="rowCenter"  v-if="isBook">
            <div class="cont" v-for="item in list">
                <div class="avatar">
                </div>
                <div class="info">
                    <p class="name">碧池</p>
                    <p class="depart">临床科 · 主任医师</p>
                    <p class="desc">
                        血液疾病、多发性骨髓瘤、干细胞移植。
                    </p>
                </div>
                <div class="tag">
                    在编
                </div>
            </div>
        </div>
        <!-- <drag-sort :imgSrcs="files" @change="onDragSortChange" /> -->
    </div>
</template>
<script>
import dragSort from '@/components/dragSort'
export default {
     components: {
        dragSort
    },
    data(){
        return {
            current:"tab1",
            itemTitle:"所有编制",
            listMenu:[
                {
                    text:"所有编制",
                    value:0
                },
                {
                    text:"在编",
                    value:1
                },
                {
                    text:"合同",
                    value:2
                },
                {
                    text:"临时",
                    value:3
                },
                {
                    text:"离职",
                    value:4
                },
                {
                    text:"退休",
                    value:5
                },
                {
                    text:"返聘",
                    value:6
                },
                {
                    text:"实习/见习",
                    value:6
                }
            ],
            value1: 1,
            num:0,
            isBook:false,
            list:[1,2,3],
            // files:['https://oa.zgchospital.com//img/icons/task.png','https://oa.zgchospital.com//img/icons/task.png','https://oa.zgchospital.com//img/icons/task.png']
            files:[
                {
                    name:"1"
                },
                {
                    name:"2"
                },
                {
                    name:"3"
                },
                {
                    name:"4"
                },
                {
                    name:"5"
                },
                {
                    name:"6"
                },
                {
                    name:"7"
                },
                {
                    name:"8"
                }
            ]
        }
    },
    methods:{
        getSwitch(){
            this.isBook = !this.isBook;
        },
        handleChangeScroll(e){
            this.current = e.mp.detail.key;
        },
        getCheck(item,index){
            this.num = index;
            this.itemTitle = item;
            this.$mp.page.selectComponent('#item').toggle();
        },
        onDragSortChange(data) {
            this.files = data
        },
    }
}
</script>
<style lang="scss">
    .wrap{
        .header{
            background: #fff;
            .nav{
                display: flex;
                // justify-content: space-between;
                align-items: center;
                padding-left: 33rpx;
                // position: relative;
                border-bottom: 1rpx solid #e2e4e3;
                .search{
                    input{
                        width: 181rpx;
                        height: 76rpx;
                        border-radius: 40rpx;
                        font-size: 32rpx;
                        background: #eceeed;
                        text-align: center;
                    }
                    .placeholder{
                        color: #999999;
                    }
                }
                .tabs{
                    width: 60%;
                    overflow: hidden;
                }
                .more{
                    // position: absolute;
                    // right: 0;
                    width: 94rpx;
                    height: 108rpx;
                    background: #fff;
                    box-shadow: -12rpx 0rpx 9rpx 0rpx rgba(0, 0, 0, 0.05);
                    z-index: 999;
                }
            }
            .screen{
                display: flex;
                justify-content: space-between;
                align-content: center;
                border-bottom: 1rpx solid #e2e4e3;
                .select{
                    // width: 135rpx;
                    .list{
                        .name{
                            padding-left: 33rpx;
                            line-height: 104rpx;
                            border-bottom: 1rpx solid #e2e4e3;
                            font-size: 26rpx;
                            color: #999999;
                        }
                        .name.active{
                            color: #3399ff;
                        }
                    }
                }
                .right{
                    display: flex;
                    .box{
                        width: 104rpx;
                        height: 104rpx;
                        background: #ccc;
                    }
                }
            }
        }
        .content{
            padding: 0 33rpx;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            .box_card{
                width: 48%;
                background: #fff;
                margin-top: 33rpx;
                .imgs{
                    height: 331rpx;
                    background: #ccc;
                }
                .Copywriting{
                    padding: 20rpx;
                    .name{
                        display: flex;
                        align-items: center;
                        justify-content: space-between;
                        span{
                            display: inline-block;
                        }
                        span:nth-child(1){
                            font-size: 33rpx;
                            color: #333;
                            font-weight: bold;
                        }
                        span:nth-child(2){
                            border: 5rpx solid #39c1b3;
                            font-size: 18rpx;
                            color: #39c1b3;
                            border-radius: 3rpx;
                        }
                    }
                    .depart{
                        font-size: 26rpx;
                        color: #999999;
                    }
                    .desc{
                        font-size: 26rpx;
                        color: #666666;
                        margin-top: 28rpx;
                    }
                }
            }
        }
        .rowCenter{
            .cont{
                padding: 25rpx 33rpx;
                display: flex;
                justify-content: space-between;
                background: #fff;
                margin-top: 14rpx;
                .avatar{
                    width: 83rpx;
                    height: 83rpx;
                    border-radius: 12rpx;
                    background: #ccc;
                }
                .info{
                    flex: 1;
                    margin-left: 26rpx;
                    .name{
                        font-size: 33rpx;
                        color: #333333;
                        font-weight: bold;
                    }
                    .depart{
                        font-size: 29rpx;
                        color: #999999;
                        margin-top: 10rpx;
                    }
                    .desc{
                        margin-top: 35rpx;
                        color: #666666;
                        font-size: 29rpx;
                    }
                }
                .tag{
                    height: 26rpx;
                    line-height: 26rpx;
                    border: 5rpx solid #39c1b3;
                    font-size: 18rpx;
                    color: #39c1b3;
                    border-radius: 3rpx;
                }
            }
        }
    }
</style>