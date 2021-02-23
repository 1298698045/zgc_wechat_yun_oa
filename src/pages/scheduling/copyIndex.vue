<template>
  <view class="page">
    <div>
        <Calendar
        :months="months"
        :value="value"
        @next="next"
        @prev="prev"
        :events="events"
        lunar
        :clean="false"
        @select="select"
        ref="calendar"
        arrowLeft="https://oa.zgchospital.com/img/wechat/zuojiantou.png"
        arrowRight="https://oa.zgchospital.com/img/wechat/youjiantou.png"
        @selectMonth="selectMonth"
        @selectYear="selectYear"
        :tileContent="tileContent"
        :almanacs="almanacs"
        />
    </div>
    <van-popup
        :show="show"
        position="center"
        :round="true"
        custom-style="height: auto;"
        @close="onClose"
    >
        <div class="popup_wrap">
            <div class="head_time">
                {{selDate}} {{weekDay}}
            </div>
            <div class="content">
                <div class="label">
                    当前班次：{{tagName}}
                </div>
                <div class="rowTag">
                    <p class="btn" :class="{'active':num==index}" v-for="(item,index) in tagList" :key="index" @click="getSelTag(item,index)">{{item.name}}</p>
                </div>
                <div class="textarea_wrap">
                    <textarea class="textarea" placeholder="请输入备注" :auto-height="true" name="" id="" cols="30" rows="10"></textarea>
                </div>
            </div>
            <div class="foot">
                <p class="cancel" @click="onClose">取消</p>
                <p class="save" @click="getSave">保存</p>
            </div>
        </div>
    </van-popup>
  </view>
</template>

<script scoped>
import Calendar from 'mpvue-calendar';
import 'mpvue-calendar/src/style.css';
  //index.js
  //获取应用实例
  const app = getApp()
  export default {
    data() {
      return {
            date: [],
            selected: 0,
            months: ['一月', '二月', '三月', '四月', '五月', '六月', '七月', '八月', '九月', '十月', '十一月', '十二月'],
            disabledArray: ['2021-2-1', '2018-6-25'],
            value: [2021, 2, 7],
            begin:[2016, 1, 1],
            end:[2020, 1, 1],
            events: {'2021-2-21': '今日备注', '2021-2-21': '早班'},
            // almanacs: {'9-3': '抗战胜利日', '11-17': '学生日'},
            tileContent: [
                {date: '2018-9-22', className: 'holiday', content: '休'},
                {date: '2018-9-23', className: 'holiday', content: '休'}
            ],
            show:false,
            tagList:[
                {
                    name:'白班'
                },
                {
                    name:'夜班'
                },
                {
                    name:'早班'
                },
                {
                    name:'休息'
                },
                {
                    name:'中班'
                },
                {
                    name:'空'
                }
            ],
            num:-1,
            tagName:"",
            selDate:"",
            weekDay:""
        }
    },
    components:{
        Calendar
    },
    onLoad(){
        Object.assign(this.$data,this.$options.data());
    },
    methods: {
        prev(year, month, weekIndex) {
            console.log(year, month, weekIndex)
        },
        next(year, month, weekIndex) {
            console.log(year, month, weekIndex)
        },
        selectYear(year) {
            console.log(year)
        },
        selectMonth(month, year) {
            console.log(year, month)
        },
        setToday() {
            this.$refs.calendar.setToday()
        },
        dateInfo() {
            const info = this.$refs.calendar.dateInfo(2018, 8, 23)
            console.log(info)
        },
        renderer() {
            this.$refs.calendar.renderer(2018, 8); //渲染2018年8月份
        },
        select(val,val2) {
            // console.log(val,val2)
            let date = val.join('-');
            this.selDate = date;
            let week = ['周日','周一','周二','周三','周四','周五','周六'];
            let day = new Date(date.replace(/-/g,'/')).getDay();
            this.weekDay = week[day];
            this.num = this.tagList.findIndex(item=>item.name==val2.event);
            this.show = true;
        },
        getSelTag(item,index){
            this.tagName = item.name;
            this.num = index;
        },
        onClose(){
            this.show = false;
        },
        getSave(){
            if(this.tagName==''){
                wx.showToast({
                    title:"请选择班次",
                    icon:"none",
                    duration:2000
                })
                return false;
            }else {
                this.$set(this.events,this.selDate,this.tagName);
                this.show = false;
            }
        }
    }
  }
</script>
<style scoped lang="scss">
.page{
    .popup_wrap{
        .head_time{
            background: #f4f4f4;
            font-size: 28rpx;
            color: #333;
            text-align: center;
            padding: 32rpx 0;
        }
        .content{
            padding: 32rpx;
            .rowTag{
                display: flex;
                flex-wrap: wrap;
                .btn{
                    width: 21%;
                    text-align: center;
                    border: 1rpx solid #e2e3e5;
                    padding: 20rpx 0;
                    margin-top: 20rpx;
                    margin-right: 20rpx;
                    font-size: 28rpx;
                    border-radius: 3rpx;
                }
                .btn:last-child{
                    margin-right: 0;
                }
                .btn.active{
                    border: none;
                    background: #3399ff;
                    color: #fff;
                }
            }
            .textarea_wrap{
                margin-top: 30rpx;
                .textarea{
                    min-height: 100rpx;
                    border: 1rpx solid #e2e3e5;
                    border-radius: 10rpx;
                    padding: 10rpx;
                    font-size: 28rpx;
                }
            }
        }
        .foot{
            display: flex;
            justify-content: space-between;
            border-top: 1rpx solid #e2e3e5;
            p{
                flex: 1;
                text-align: center;
                padding: 30rpx 0;
                font-size: 28rpx;
            }
            .cancel{
                color: #666666;
                border-right: 1rpx solid #e2e3e5;
            }
            .save{
                color: #3399ff;
            }
        }
    }
}
</style>