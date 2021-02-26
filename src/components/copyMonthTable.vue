<template>
    <div class="wrap">
        <div class="table">
            <div class="date">{{date}}</div>
            <div class="tr bg-w">
                <p class="th">日</p>
                <p class="th">一</p>
                <p class="th">二</p>
                <p class="th">三</p>
                <p class="th">四</p>
                <p class="th">五</p>
                <p class="th">六</p>
            </div>
            <swiper
                :current="current"
                @change="getCurrent"
                slidesPerView="2"
                :autoplay="false"
                class="swiper_wrap"
            >
                <block>
                    <swiper-item class="items" v-for="(item,idx) in currentList" :key="idx">
                        <div class="tr bg-g">
                            <div class="td" v-for="(v,index) in item" :key="index" @click="v.subject?getDetail(v):''">
                                <div class="times"> 
                                    <p class="day">{{v.day}}</p>
                                    <p class="lunar" :class="{'active':v.Sat}" v-if="v.lunar||v.Sat">{{v.Sat?v.Sat:v.lunar.IDayCn}}</p>
                                </div>
                                <p class="cont_a" v-if="v.day">
                                    <span class="radius gre" v-if="v.subject"></span>
                                    <span class="text">{{v.subject}}</span>
                                </p>
                                <div class="td_active" v-if="v.subject">

                                </div>
                            </div>
                        </div>
                    </swiper-item>
                </block>
            </swiper>
        </div>
    </div>
</template>
<script>
import calendar from '@/utils/Calendar-lunar';
export default {
    name:'MonthTable',
    props:['listData'],
    data(){
        return {
            list:[],
            date:"",
            current:1,
            currentList:[],
            currDay:"",
            endDay:""
        }
    },
    watch:{
        listData(val){
            console.log(val,'watch')
            this.list = val;
        }
    },
    onLoad(){
        // console.log(this.getCurrentMonthFirst(),this.getCurrentMonthLast());
        let date = new Date();
        let y = date.getFullYear() 
        let m = date.getMonth()+1;
        let d = date.getDate();
        this.date = `${y}-${m}-${d}`;
        let currDay = new Date(this.getCurrentMonthFirst()).getDay();
        let endDay = new Date(this.getCurrentMonthLast()).getDate();
        this.currDay = currDay;
        this.endDay = endDay;
        // this.now();
    },
    mounted(){
        this.$nextTick(()=>{
            setTimeout(() => {
                console.log(this.$parent.list,'0-0-00-0-0-')
                this.list = this.$parent.list;
                this.now();
            }, 1000);
        })
    },
    methods:{
        getCurrent(e){
            // console.log(e,'--------------');
            let current = e.mp.detail.current;
            if(current == this.currentList.length-1){
                this.current = current;
                let date = this.currentList[current][0].date;
                let d = this.getNextMonth(date);
                console.log(d,'dddddddddd')
                let nT = d.split('-');
                let start = this.getMonthStartDate(nT[0],nT[1]);
                console.log(start,'start')
                let dayNum = this.getMonthDays(new Date(date.replace(/-/g,'/')).getFullYear(),new Date(date.replace(/-/g,'/')).getMonth());
                let currDay = new Date(start).getDay();
                console.log(currDay,dayNum,'8888')
                let next = this.setDate(currDay,dayNum,d);
                console.log(this.currentList[current][0].date,'wljhkfhkdk')
                // this.currentList[current].forEach(item=>{
                    //     this.$set(item,'d','123123')
                // })
                this.currentList.push(next);
                this.date = date;
                setTimeout(() => {
                    this.getAddList(currDay,dayNum)
                }, 100);
                
            }else if(current==0){
                this.current = current;
                console.log(this.current,'this.current')
                let date = this.currentList[this.current][0].date;
                console.log(date,'prepre');
                let d2 = this.getPreMonth(date);
                let nT2 = d2.split('-');
                let start2 = this.getMonthStartDate(nT2[0],nT2[1]);
                let dayNum2 = this.getMonthDays(new Date(date.replace(/-/g,'/')).getFullYear(),new Date(date.replace(/-/g,'/')).getMonth());
                let currDay2 = new Date(start2).getDay();
                console.log(currDay2,dayNum2)
                let pre = this.setDate(currDay2,dayNum2,d2);
                console.log(pre,'pre')
                this.currentList.unshift(pre);
                this.current = 1;
                // console.log(pre,this.currentList,'000')
                this.date = date;
                setTimeout(() => {
                    this.getAddList(currDay2,dayNum2)
                }, 100);
            }else {
                this.date = this.currentList[current][0].date;
            }
            this.$emit('change',{
                date:this.date,
                current:current
            })
        },
        onday(){
            let date = new Date(this.date.replace(/-/g,'/')); //获取日期  日期是2020-07 写成你自己的
            let CurrentDate = "";
            let year = date.getFullYear();
            let month = (date.getMonth()) < 10 ? "0" +(date.getMonth()) : (date.getMonth()); //自动补0
            // console.log(year);
            // console.log(month);
            if(month<1){
                CurrentDate = (year - 1) + "-" + "12"
            }else{
                CurrentDate = year + "-" + month
            }
            return CurrentDate;
        },
        nextday(){
            let date = new Date(this.date.replace(/-/g,'/'));
            let CurrentDate = "";
            let year = date.getFullYear();
            let month = (date.getMonth() + 1 + 1) < 10 ? "0" +(date.getMonth() + 1 + 1) : (date.getMonth() + 1 + 1);
            if(month>12){
                CurrentDate = (year + 1) + "-" + "01"
            }else{
                CurrentDate = year + "-" + month
            }
            return CurrentDate;
        },
        now(){
            let d = this.getPreMonth(this.date);
            let preTemp = d.split('-');
            console.log(preTemp,'prossdf')
            let dayNum = this.getMonthDays(new Date(this.date.replace(/-/g,'/')).getFullYear(),new Date(this.date.replace(/-/g,'/')).getMonth());
            let d2 = this.getNextMonth(this.date);
            let nextTemp = d2.split('-');
            let start = this.getMonthStartDate(preTemp[0],preTemp[1]);
            let end = this.getMonthEndDate(preTemp[0],preTemp[1]);
            let nStart = this.getMonthStartDate(nextTemp[0],nextTemp[1]);
            let currDay = new Date(start).getDay();
            let endDay = new Date(preTemp[0],preTemp[1],0).getDate(); // 获取上月共有多少天
            let currDay2 = new Date(nStart).getDay();
            let pre = this.setDate(currDay,endDay,d);
            let now = this.setDate(this.currDay,this.endDay,this.date);
            let next = this.setDate(currDay2,dayNum,d2);
            this.currentList = [pre,now,next];
            this.getAddList(this.currDay,this.endDay);
            
        },
        getAddList(currDay,len){
            console.log(this.list,this.current,'listlist')
            for(let i=0;i<this.list.length;i++){
                console.log(this.list[i].scheduledStart,this.list[i].scheduledEnd);
                let diffDays = this.getComputation(this.list[i].scheduledStart,this.list[i].scheduledEnd)
                let isStart = this.getIsCurrMonth(this.list[i].scheduledStart);
                let isEnd = this.getIsCurrMonth(this.list[i].scheduledEnd);
                console.log(isStart,isEnd,'sljkdl');
                if(!isStart&&isEnd){
                   let starIdx = this.currentList[this.current].findIndex(one=>one.currDate?this.isToday(one.currDate,this.list[i].scheduledStart):'');
                    let endIdx = this.currentList[this.current].findIndex(one=>one.currDate?this.isToday(one.currDate,this.list[i].scheduledEnd):'');
                    //    debugger
                    if(starIdx==endIdx){
                        this.currentList[this.current][starIdx].subject = this.list[i].subject;
                        this.currentList[this.current][starIdx].isShow = false;
                    }
                    for(var q=currDay;q<endIdx;q++){
                        console.log(this.currentList[this.current][q],'this.currentList[this.current][q]')
                        this.currentList[this.current][q].subject = this.list[i].subject;
                        this.currentList[this.current][q].isShow = false;
                    }
                }else if(isStart&&isEnd){
                    console.log(this.currentList[this.current],'----------this.currentList[this.current]');
                    let starIdx = this.currentList[this.current].findIndex(one=>one.currDate?this.isToday(one.currDate,this.list[i].scheduledStart):'');
                    let endIdx = this.currentList[this.current].findIndex(one=>one.currDate?this.isToday(one.currDate,this.list[i].scheduledEnd):'');
                    //    debugger
                    if(starIdx==endIdx){
                        this.currentList[this.current][starIdx].subject = this.list[i].subject;
                        this.currentList[this.current][starIdx].isShow = false;
                    }
                    // for(var q=starIdx;q<endIdx+1;q++){
                    //     console.log(this.currentList[this.current][q],'-------this.currentList[this.current][q]')
                    //     this.currentList[this.current][q].subject = this.list[i].subject;
                    // }
                }
                else if(isStart&&!isEnd){
                    let index = this.currentList[this.current].findIndex(one=>one.currDate?this.isToday(one.currDate,this.list[i].scheduledStart):'');
                    console.log(index,currDay,'index')
                    for(var q=index;q<len;q++){
                        console.log(this.currentList[this.current][q],'this.currentList[this.current][q]')
                        this.currentList[this.current][q].subject = this.list[i].subject;
                        this.currentList[this.current][q].isShow = false;
                    }
                }
                console.log(this.currentList,'currentlist')
                // this.currentList[this.current].forEach(item=>{
                //     console.log(item,'-------------------------0000')
                //     let day = item.currDate;
                // })
            }
        },
        isToday(str,date){
            console.log(str,date);
            date = date.split(' ')[0];
            var d = new Date(str.replace(/-/g,"/"));
            var todaysDate = new Date(date.replace(/-/g,"/"));
            if(d.setHours(0,0,0,0) == todaysDate.setHours(0,0,0,0)){
                return true;
            } else {
                return false;
            }
        },
        // 判断是否是当前月
        getIsCurrMonth(ADate){
            let now=new Date(this.date.replace(/-/g,'/'));
            // let today=now.toISOString().substring(0,7);
            let today = now.getMonth()+1;
            console.log('判断本月:');
            console.log('本月日期:'+today);
            ADate = new Date(ADate.replace(/-/g,'/')).getMonth()+1;
            if(today==ADate){
                console.log('ADate本月');
                return true;
            }else{
                console.log('ADate非本月');
                return false;
            }
        },
        getComputation(d1,d2){
            var dateBegin = new Date(d1.replace(/-/g,'/'));//将-转化为/，使用new Date
            var dateEnd = new Date(d2.replace(/-/g,'/'));//将-转化为/，使用new Date
            var beginDiff = dateEnd.getTime() - dateBegin.getTime();//时间差的毫秒数
            var beginDayDiff = Math.floor(beginDiff / (24 * 3600 * 1000));//计算出相差天数
            console.log(beginDiff,beginDayDiff,'-')
            return beginDayDiff;
        },
        setDate(currDay,endDay,date){
            // for(let i=0;i<this.list.length;i++){
            //     console.log(this.list[i].scheduledStart,endDay,date,'-');
            // }
            let myDate = new Date(date.replace(/-/g,'/'));
            let y = myDate.getFullYear();
            let m = myDate.getMonth()+1;
            let temp = [];
            for(var i=0; i<currDay; i++){
                temp.push({
                    date:date
                });
            }
            for(var j=0;j<endDay;j++){
                temp.push({
                    day:j+1,
                    date:date,
                    currDate:`${y}-${m}-${j+1}`,
                    month:m,
                    lunar:calendar.calendar.solar2lunar(y,m,j+1)
                });
            }
            temp.length = 42;
            let num = 42 - endDay - (currDay);
            if(num>14){
                temp.splice(temp.length-14,temp.length);
            }else if(num>7){
                temp.splice(temp.length-7,temp.length);
            }
            let lastNum = temp.length-endDay-currDay;
            temp.splice(endDay+currDay,lastNum);
            for(var i=0; i<lastNum; i++){
                temp.push({
                    date:date
                })
            }
            return temp;
        },
        // 获取当月的第一天
        getCurrentMonthFirst(){
            var date = new Date();
            date.setDate(1);
            var month = parseInt(date.getMonth()+1);
            var day = date.getDate();
            if (month < 10) {
                month = '0' + month
            }
            if (day < 10) {
                day = '0' + day
            }
            return date.getFullYear() + '-' + month + '-' + day;
        },
        // 获取当月的最后一天
        getCurrentMonthLast(){
            var date=new Date();
            var currentMonth=date.getMonth();
            var nextMonth=++currentMonth;
            var nextMonthFirstDay=new Date(date.getFullYear(),nextMonth,1);
            var oneDay=1000*60*60*24;
            var lastTime = new Date(nextMonthFirstDay-oneDay);
            var month = parseInt(lastTime.getMonth()+1);
            var day = lastTime.getDate();
            if (month < 10) {
                month = '0' + month
            }
            if (day < 10) {
                day = '0' + day
            }
            return date.getFullYear() + '-' + month + '-' + day;
        },
        //获取上个月日期
        getLastMonth(){
            var date = new Date(this.date.replace(/-/g,'/')); 
            var year = date.getFullYear();
            var month = date.getMonth();
            if(month == 0){
                year = year -1;
                month = 12; 
            }
            return year+"-"+month+"-";
        },
        formatDate(date) {　　　 
        　 var myyear = date.getFullYear();　　 
        　 var mymonth = date.getMonth();
        　 var myweekday = date.getDate();　　　 
        　　　　 
        　 if(mymonth < 10){　　 
        　　　 mymonth = "0" + mymonth;　　 
        　 }　　　 
        　 if(myweekday < 10){　　 
        　　　 myweekday = "0" + myweekday;　　 
        　 }　　 
            if(mymonth =='00'){
                mymonth = '01';
            }
        　 return (myyear+"-"+mymonth + "-" + myweekday);　　　 
        },
        //获得某月的开始日期　　 
        getMonthStartDate(paraYear,paraMonth){　　
            console.log(paraYear, paraMonth,'paraYear, paraMonth') 
        　 var monthStartDate = new Date(paraYear, paraMonth, 1);　　
        console.log(this.formatDate(monthStartDate),'this.formatDate(monthStartDate)')　 
        　 return this.formatDate(monthStartDate);　　 
        },     　　 
        //获得某月的结束日期　　 
        getMonthEndDate(paraYear,paraMonth){ 
        　 var monthEndDate = new Date(paraYear,paraMonth, this.getMonthDays(paraYear,paraMonth));　　　 
        　 return this.formatDate(monthEndDate);　　 
        },
        //获得某月的天数　　 
        getMonthDays(paraYear,paraMonth){
　         var monthStartDate = new Date(paraYear, paraMonth, 1);　　　 
        　 var monthEndDate = new Date(paraYear, paraMonth + 1, 1);　　　 
        　 var　 days　 =　 (monthEndDate　 -　 monthStartDate)/(1000　 *　 60　 *　 60　 *　 24);　　　 
        　 return days;　
        },
        getPreMonth(date) {
            var arr = date.split('-');
            var year = arr[0]; //获取当前日期的年份
            var month = arr[1]; //获取当前日期的月份
            var day = arr[2]; //获取当前日期的日
            var days = new Date(year, month, 0);
            days = days.getDate(); //获取当前日期中月的天数
            var year2 = year;
            var month2 = parseInt(month) - 1;
            if (month2 == 0) {
                year2 = parseInt(year2) - 1;
                month2 = 12;
            }
            var day2 = day;
            var days2 = new Date(year2, month2, 0);
            days2 = days2.getDate();
            if (day2 > days2) {
                day2 = days2;
            }
            if (month2 < 10) {
                month2 = '0' + month2;
            }
            var t2 = year2 + '-' + month2 + '-' + day2;
            return t2;
        },
        /**
         * 获取下一个月
         *
         * @date 格式为yyyy-mm-dd的日期，如：2014-01-25
         */        
        getNextMonth(date) {
            var arr = date.split('-');
            var year = arr[0]; //获取当前日期的年份
            var month = arr[1]; //获取当前日期的月份
            var day = arr[2]; //获取当前日期的日
            var days = new Date(year, month, 0);
            days = days.getDate(); //获取当前日期中的月的天数
            var year2 = year;
            var month2 = parseInt(month) + 1;
            if (month2 == 13) {
                year2 = parseInt(year2) + 1;
                month2 = 1;
            }
            var day2 = day;
            var days2 = new Date(year2, month2, 0);
            days2 = days2.getDate();
            if (day2 > days2) {
                day2 = days2;
            }
            if (month2 < 10) {
                month2 = '0' + month2;
            }
        
            var t2 = year2 + '-' + month2 + '-' + day2;
            return t2;
        }, 
        getDetail(v){
            v.isShow = true;
        }
    }
}
</script>
<style lang="scss">
page{
    // background: #fff !important;
}
.wrap{
    padding-bottom: 80px;
    .table {
        border: 0px solid darkgray;
        background: #fff;
        .date{
            text-align: center;
            font-weight: bold;
            color: #333333;
            font-size: 30rpx;
            padding: 10rpx 0;
        }
        .tr {
            display: flex;
            width: 100%;
            align-items: center;
            .th{
                flex: 1;
                text-align: center;
                font-size: 24rpx;
                color: #7b8187;
            }
        }
        .bg-w{
            padding: 15rpx 0 25rpx 0;
            border-bottom: 1rpx solid #e2e3e5;
        }
        .swiper_wrap{
            height: 600px !important;
        }
        .bg-g{
            flex-wrap: wrap;
            .td{
                width: 14.27%;
                height: 200rpx;
                // background: red;
                border-right: 1rpx solid #e2e3e5;
                border-bottom: 1rpx solid #e2e3e5;
                box-sizing: border-box;
                position: relative;
                .times{
                    display: flex;
                    justify-content: space-between;
                    // text-align: center;
                    padding:10rpx 10rpx 0 10rpx;
                    .day{
                        font-size: 33rpx;
                        color: #333333;
                        font-weight: bold;
                    }
                    .lunar{
                        width: 30rpx;
                        font-size: 24rpx;
                        color: #999999;
                    }
                    .lunar.active{
                        color: #ff4444;
                    }
                }
                .cont_a{
                    padding: 5rpx 10rpx;
                    display: flex;
                    align-items: center;
                    white-space: normal;
                    word-break: break-all;
                    overflow: hidden;
                    .radius{
                        width: 8rpx;
                        height: 8rpx;
                        border-radius: 5rpx;
                        background: #52b55e;
                        display: inline-block;
                    }
                    .text{
                        font-size: 22rpx;
                        color: #333333;
                        margin-left: 5rpx;
                        width: 70rpx;
                        white-space: nowrap;
                        overflow: hidden;
                        text-overflow: ellipsis;
                    }
                }
                .td_active{
                    position: absolute;
                    width: 100%;
                    height: 100%;
                    background: rgba(0,0,0,.5);
                    bottom: 0;
                }
            }
        }
    }
}
</style>