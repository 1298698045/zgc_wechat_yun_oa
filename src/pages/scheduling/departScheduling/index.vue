<template>
    <div class="wrap">
        <div class="header">
            <div class="year"></div>
            <div class="andTime">
                <div class="leftIcon" @click="getPrevTime">
                    <i-icon type="return" size="24" />
                </div>
                <div class="cont">
                    <span class="start_time">{{timeList[0].time}}</span>
                    至
                    <span class="end_time">{{timeList[6].time}}</span>
                </div>
                <div class="rightIcon" @click="getNextTime">
                    <i-icon type="enter" size="24" />
                </div>
            </div>
        </div>
        <div class="table">
            <div class="tr tr_head">
                <div class="td td_name">
                    姓名
                </div>
                <div class="td">
                    <p class="day">一</p>
                    <p class="time">
                        {{timeList[0].date}}
                    </p>
                </div>
                <div class="td">
                    <p class="day">二</p>
                    <p class="time">
                        {{timeList[1].date}}
                    </p>
                </div>
                <div class="td">
                    <p class="day">三</p>
                    <p class="time">
                        {{timeList[2].date}}
                    </p>
                </div>
                <div class="td">
                    <p class="day">四</p>
                    <p class="time">
                        {{timeList[3].date}}
                    </p>
                </div>
                <div class="td">
                    <p class="day">五</p>
                    <p class="time">
                        {{timeList[4].date}}
                    </p>
                </div>
                <div class="td">
                    <p class="day">六</p>
                    <p class="time">
                        {{timeList[5].date}}
                    </p>
                </div>
                <div class="td">
                    <p class="day">日</p>
                    <p class="time">
                        {{timeList[6].date}}
                    </p>
                </div>
            </div>
            <div class="tr tr_cont" id="table_tr" v-for="(item,index) in list" :key="index">
                <div class="td td_name_cont" :style="{'height':tdHeight+'px','line-height':tdHeight+'px'}" @click="getCheck(1)">
                    {{item.Name}}
                </div>
                <div class="td tds" v-for="(v,idx) in item.AttendData" :key="idx" :style="{'height':tdHeight+'px'}">
                    <p>
                        <span v-for="(self,i) in v.Shifts" :key="i">{{self.WorkShiftIdName}}</span>
                    </p>
                </div>
                <!--
                    <div class="td" :style="{'height':tdHeight+'px'}"></div>
                    <div class="td" :style="{'height':tdHeight+'px'}"></div>
                    <div class="td" :style="{'height':tdHeight+'px'}"></div>
                    <div class="td" :style="{'height':tdHeight+'px'}"></div>
                    <div class="td" :style="{'height':tdHeight+'px'}"></div>
                    <div class="td" :style="{'height':tdHeight+'px'}"></div>
                    <div class="td" :style="{'height':tdHeight+'px'}"></div>
                -->
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return {
            tdHeight:"",
            currentList:[],
            activeIdx:0,
            list:[
                {
                    AttendDate:[]
                }
            ]
        }
    },
    computed:{
        timeList(){
            let temp = [];
            this.currentList.forEach(item=>{
                let newDate = new Date(item.replace(/-/g,'/'));
                const time = newDate.getDate() >= 10 ? newDate.getDate() : '0'+newDate.getDate();
                temp.push({
                    time:item,
                    date:time
                })
            })
            return temp;
        },
        sessionkey(){
            return wx.getStorageSync('sessionkey')
        },
        unitId(){
            return wx.getStorageSync('businessUnitId');
        },
        startTime(){
            return this.startTime = this.currentList[0];
        },
        endTime(){
            return this.startTime = this.currentList[this.currentList.length-1];
        }
    },
    onLoad(){
        wx.createSelectorQuery().select('.td').boundingClientRect(rect=>{
          this.tdHeight = rect.width;
        }).exec();
        let date = new Date();
        let currentTime = date.getTime();
        this.currentList = this.getDates(currentTime);
        this.getQuery();
        // this.startTime = this.
        // this.currentList.forEach(item=>{
        //     let newDate = new Date(item.replace(/-/g,'/'));
        //     const time = newDate.getDate() >= 10 ? newDate.getDate() : '0'+newDate.getDate();
        //     this.timeList.push({
        //         time:item,
        //         date:time
        //     })
        // })
    },
    methods:{
        getQuery(){
            this.$httpWX.get({
                url:this.$api.message.queryList,
                data:{
                    SessionKey:this.sessionkey,
                    method:this.$api.scheduling.departQuery,
                    unitId:this.unitId,
                    startDate:this.startTime,
                    endDate:this.endTime,
                    unitType:10
                }
            }).then(res=>{
                this.list = res.listData;
            })
        },
        getDates(currentTime) {//JS获取当前周从星期一到星期天的日期
            var currentDate = new Date(currentTime)
            var timesStamp = currentDate.getTime();
            var currenDay = currentDate.getDay();
            var dates = [];
            for (var i = 0; i < 7; i++) {
                dates.push(new Date(timesStamp + 24 * 60 * 60 * 1000 * (i - (currenDay + 6) % 7)).toLocaleDateString().replace(/\//g, '-'));
            }
            return dates
        },
        // 上一周
        getPrevTime(){
            let startTime = new Date(this.timeList[0].time.replace(/-/g,'/')).getTime() - 7 * 24 * 60 * 60 * 1000;
            this.currentList = this.getDates(startTime);
            this.getQuery();
        },
        // 下一周
        getNextTime(){
            let startTime = new Date(this.timeList[0].time.replace(/-/g,'/')).getTime() + 7 * 24 * 60 * 60 * 1000;
            this.currentList = this.getDates(startTime);
            this.getQuery();
        },
        getCheck(index){
            this.num = index;
        }
    }
}
</script>
<style lang="scss">
    .wrap{
        width: 100%;
        height: 100%;
        .header{
            display: flex;
            justify-content: center;
            align-items: center;
            background: #fff;
            padding: 20rpx 0;
            border-bottom: 1rpx solid #e2e3e5;
            .andTime{
                display: flex;
                align-items: center;
                .cont{
                    padding: 0 20rpx;
                }
            }
        }
        .table{
            width: 100%;
            font-size: 28rpx;
            .tr{
                display: flex;
                align-items: center;
                border-bottom: 1px solid #e2e3e5;
                .td_name{
                    background: #ccc;
                }
                .td{
                    flex: 1;
                    text-align: center;
                }
            }
            .tr_head{
                height: 130rpx;
                .td_name{
                    line-height: 130rpx;
                }
                .td{
                    .time{
                        margin-top: 25rpx;
                    }
                }
            }
            .tr_cont{
                .td{
                    box-sizing: border-box;
                    border-right: 1rpx solid #e2e3e5;
                }
                .td:nth-child(8){
                    border: none;
                }
                .tds{
                    display:flex;
                    justify-content:center;
                    align-items:center;
                    span{
                        font-size:20rpx;
                        display:inline-block;
                    }
                }
                // .td.active_td{
                //     border: 1px solid #333333;
                // }
            }
        }
    }
</style>