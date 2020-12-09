<template>
  <div class="wrap">
    <picker @change="pickerLeave" :value="leaveIdx" range-key="name" :range="leaveList">
      <div class="rowWrap">
        <p class="label">
          请假类型
          <span>*</span>
        </p>
        <p class="name">
          <span
            :class="leaveList[leaveIdx]?'active':''"
          >{{leaveList[leaveIdx]?leaveList[leaveIdx].name:'请选择'}}</span>
          <i-icon type="enter" color="#cccccc" />
        </p>
      </div>
    </picker>
    <h3>假期余额</h3>
    <div class="content">
      <picker
        @change="pickerStartTime"
        mode="multiSelector"
        :value="multiIndex"
        :range="newMultiArray"
      >
        <div class="row">
          <p class="label">
            开始时间
            <span>*</span>
          </p>
          <p class="name">
            <span>{{startTime}}</span>
            <i-icon type="enter" color="#cccccc" />
          </p>
        </div>
      </picker>
      <picker
        @change="pickerEndTime"
        mode="multiSelector"
        :value="endMultiIndex"
        :range="newMultiArray"
      >
        <div class="row">
          <p class="label">
            结束时间
            <span>*</span>
          </p>
          <p class="name">
            <span>{{endTime}}</span>
            <i-icon type="enter" color="#cccccc" />
          </p>
        </div>
      </picker>
      <div class="row">
        <p class="label">
          时长(天)
          <span>*</span>
        </p>
        <p class="name">
          <input
            type="text"
            placeholder-style="text-align:right;color: #ababab;"
            selection-end="-1"
            placeholder="请输入时长"
          />
        </p>
      </div>
      <div class="rowBottom">
        <p>
          根据排班自动计算时长
          <span>查看明细</span>
        </p>
      </div>
    </div>
    <div class="leaveComment">
      <p>
        请假事由
        <span>*</span>
      </p>
      <div class="box">
        <textarea name id cols="30" rows="10"></textarea>
      </div>
    </div>
    <div class="imgContent">
      <div class="head" @click="handleSelPhoto">
        <p>图片</p>
        <p>
          <i class="iconfont icon-tupian"></i>
        </p>
      </div>
      <div class="cont" v-if="imgList!=''">
        <p class="imgWrap" v-for="(item,index) in imgList" :key="index">
          <img :src="item" alt />
          <span class="close" @click="getCloseImg(index)">
            <i-icon type="close" color="#fff" size="12" />
          </span>
        </p>
      </div>
    </div>
    <!-- 审批人 -->
    <div class="approved">
      
    </div>
    <div class="footer" :class="{'bottomActive':isModelmes,'footImt':!isModelmes}">
        <div class="btn">
            <van-button type="info" block @click="getSubmit">提交</van-button>
        </div>
    </div>
    <van-popup
            :show="agreeShow"
            position="bottom"
            custom-style="width:100%;height: auto;"
            @close="onCloseAgree"
            overlay-style="background: #333;opacity: .5;">
            <div class="agreeWrap">
                <div class="header">
                    <div>
                        <p class="radius">{{createdByName}}</p>
                    </div>
                    <div>
                        <div class="h3">{{createdByName}}提交的流程申请表</div>
                        <p><span>标题：</span>生物医学研究伦理审查审批表</p>
                    </div>
                </div>  
                <div class="cont">
                    <div v-for="(item,index) in stepList" :key="index">
                        <h3>
                            <van-checkbox use-icon-slot custom-class="checkbox" :name="item.TransitionId" :value="item.Selected" @change="(e)=>{changeAll(e,item)}">
                                
                                {{item.ToActivityName}}
                                <div class="slot" slot="icon" v-if="item.Selected">
                                    <p></p>
                                </div>
                                <div class="default" slot="icon" v-else></div>
                            </van-checkbox>
                        </h3>
                        <div class="box">
                            <div class="row" v-for="(v,i) in item.ParticipantMember" :key="i">
                                <van-checkbox :name="v.UserId" :value="v.Selected" @change="(e)=>{changeItem(e,item,v)}">{{v.FullName}}/{{v.BusinessUnitIdName}}</van-checkbox>
                            </div>
                            <div class="rows">
                                <p class="add" @click="getAddPeople(item)">
                                    <i-icon type="add" size="30" color="#3399ff" />
                                    <span>
                                        添加办理人员
                                    </span>
                                </p>
                            </div>
                        </div>
                    </div>
                    <div class="popupRow">
                        <div class="row">
                            <p>通知</p>
                            <p>
                                <span>应用内</span>
                                <span>&nbsp;短信</span>
                            </p>
                        </div>
                        <div class="text">
                            流程事务:[ 05 绍兴第二医院医共体分院招标关于采购事项的审批单 信息中心 崔曼 ]，请您查阅！
                        </div>
                    </div>  
                    <div class="textarea">
                        <p>留言</p>
                        <textarea v-model="description" name="" placeholder="请输入内容" id="" cols="30" rows="10"></textarea>
                    </div>
                </div>
                <div class="fot" :class="{'bottomActive':isModelmes,'footImt':!isModelmes}">
                    <div class="box">
                        <p @click="onCloseAgree">取消</p>  
                        <!-- <p>上一环节</p> -->
                        <p @click="getSubmit">提交</p>  
                    </div>
                </div> 
            </div>
        </van-popup>
  </div>
</template>
<script>
import { getTotal } from '@/utils/iDays';
export default {
  data() {
    return {
      leaveIdx: "",
      leaveList: [
        {
          id: "",
          name: "年假"
        },
        {
          id: "",
          name: "事假"
        },
        {
          id: "",
          name: "病假"
        },
        {
          id: "",
          name: "调休"
        },
        {
          id: "",
          name: "产假/公休"
        },
        {
          id: "",
          name: "陪产假"
        },
        {
          id: "",
          name: "婚假"
        },
        {
          id: "",
          name: "工伤假"
        },
        {
          id: "",
          name: "丧假"
        },
        {
          id: "",
          name: "哺乳假"
        },
        {
          id: "",
          name: "计生假"
        },
        {
          id: "",
          name: "探亲假"
        }
      ],
      multiIndex: [0, 0, 0, 0, 0],
      endMultiIndex: [0, 0, 0, 0, 0],
      startTime: "",
      endTime: "",
      imgList: [],
      agreeShow:false,
      stepList:[],
      createdByName:"测试"
    };
  },
  computed: {
    newMultiArray: () => {
      let array = [];
      const date = new Date();
      const years = [];
      const months = [];
      const days = [];
      const hours = [];
      const minutes = [];
      var d = new Date();
      var y = d.getFullYear();
      for (let i = y; i <= date.getFullYear() + 10; i++) {
        years.push("" + i + "年");
      }
      array.push(years);
      for (let i = 1; i <= 12; i++) {
        if (i < 10) {
          i = "0" + i;
        }
        months.push("" + i + "月");
      }
      array.push(months);
      for (let i = 1; i <= 31; i++) {
        if (i < 10) {
          i = "0" + i;
        }
        days.push("" + i + "日");
      }
      array.push(days);
      for (let i = 0; i < 24; i++) {
        if (i < 10) {
          i = "0" + i;
        }
        hours.push("" + i + "时");
      }
      array.push(hours);
      for (let i = 0; i < 60; i++) {
        if (i < 10) {
          i = "0" + i;
        }
        minutes.push("" + i + "分");
      }
      array.push(minutes);
      return array;
    },
    getNowFormatDate() {
      var date = new Date();
      var seperator1 = "-";
      var seperator2 = ":";
      var month = date.getMonth() + 1;
      var strDate = date.getDate();
      let minute = date.getMinutes();
      if (month >= 1 && month <= 9) {
        month = "0" + month;
      }
      if (strDate >= 0 && strDate <= 9) {
        strDate = "0" + strDate;
      }
      if (minute >= 1 && minute <= 9) {
        minute = "0" + month;
      }
      var currentdate =
        date.getFullYear() +
        seperator1 +
        month +
        seperator1 +
        strDate +
        " " +
        date.getHours() +
        seperator2 +
        minute;
      return currentdate;
    },
    isModelmes(){
      return wx.getStorageSync('isModelmes');
    },
    time(){
      let date = new Date();
      let y = date.getFullYear();
      let m = date.getMonth()+1;
      let day = date.getDate();
      let h = date.getHours();
      let min = date.getMinutes();
      let s = date.getSeconds();
      return `${y}-${m}-${day} ${h}:${min}:${s}`;
    }
  },
  onLoad(){
    this.getCurrent();
  },
  methods: {
    getCurrent(){
        let date = new Date(this.time.replace(/-/g,'/'));
        let years = date.getFullYear();
        let month = date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1):date.getMonth()+1;
        let d = date.getDate()<10?'0'+date.getDate():date.getDate();
        let hours = date.getHours()<10?'0'+date.getHours():date.getHours();
        let minutes = date.getMinutes()<10?'0'+date.getMinutes():date.getMinutes();

        const yIdx = this.newMultiArray[0].findIndex(item=>item==years+'年');
        const mIdx = this.newMultiArray[1].findIndex(item=>item==month+'月');
        const dIdx = this.newMultiArray[2].findIndex(item=>item==d+'日');
        const hIdx = this.newMultiArray[3].findIndex(item=>item==hours+'时');
        const minIdx = this.newMultiArray[4].findIndex(item=>item==minutes+'分');
        this.multiIndex[0] = yIdx;
        this.multiIndex[1] = mIdx;
        this.multiIndex[2] = dIdx;
        this.multiIndex[3] = hIdx;
        this.multiIndex[4] = minIdx;
        
        this.endMultiIndex[0] = yIdx;
        this.endMultiIndex[1] = mIdx;
        this.endMultiIndex[2] = dIdx;
        this.endMultiIndex[3] = hIdx;
        this.endMultiIndex[4] = minIdx;
    },
    pickerLeave(e) {
      this.leaveIdx = e.mp.detail.value;
    },
    pickerStartTime(e) {
      this.multiIndex = e.target.value;
      const index = this.multiIndex;
      const year = this.newMultiArray[0][index[0]];
      const month = this.newMultiArray[1][index[1]];
      const day = this.newMultiArray[2][index[2]];
      const hour = this.newMultiArray[3][index[3]];
      const minute = this.newMultiArray[4][index[4]];
      this.startTime =
        year + "-" + month + "-" + day + " " + hour + ":" + minute;
      this.startTime = this.RemoveChinese(this.startTime) + ":00";
    },
    pickerEndTime(e) {
      this.endMultiIndex = e.target.value;
      const index = this.endMultiIndex;
      const year = this.newMultiArray[0][index[0]];
      const month = this.newMultiArray[1][index[1]];
      const day = this.newMultiArray[2][index[2]];
      const hour = this.newMultiArray[3][index[3]];
      const minute = this.newMultiArray[4][index[4]];
      this.endTime = year + "-" + month + "-" + day + " " + hour + ":" + minute;
      this.endTime = this.RemoveChinese(this.endTime) + ":00";
      let iDays = getTotal({
          startWorkTime: 9,
          endWorkTime: 18,
          beginAt: this.startTime,
          endAt: this.endTime,
          startFreeTime: 12,
          endFreeTime: 13.5,
          excludeFreeTime: true,
          excludeDates: []
        });
      console.log(iDays,'iDays');
    },
    // 正则去除汉字
    RemoveChinese(strValue) {
      if (strValue != null && strValue != "") {
        var reg = /[\u4e00-\u9fa5]/g;
        return strValue.replace(reg, "");
      } else return "";
    },
    // 打开本地图库
    handleSelPhoto() {
      wx.chooseImage({
        count: 9,
        sizeType: ["original", "compressed"],
        sourceType: ["album", "camera"],
        success: res => {
          console.log(res);
          // tempFilePath可以作为img标签的src属性显示图片
          const tempFilePaths = res.tempFilePaths;
          tempFilePaths.forEach(item => {
            this.imgList.push(item);
          });
          // wx.uploadFile({
          //     url: "http://112.126.75.65:10020/rest?method="+'file.attachfiles.upload'+'&SessionKey=' + '5d884846-1d19-449d-b4e2-1b5a83623a41'+'&pid='+this.uuid, //仅为示例，非真实的接口地址
          //     filePath: tempFilePaths[0],
          //     name: 'file',
          //     formData: {
          //         'user': 'test'
          //     },
          //     success (res){
          //         console.log(res);
          //         const data = res.data
          //         //do something
          //     }
          // })
        }
      });
    },
    getCloseImg(index){
      this.imgList.splice(index,1);
    },
    getSubmit(){
      this.agreeShow = true;
      this.$httpWX.get({
            url:this.$api.message.queryList,
            data:{
                method: 'process.step.transition.getlist',
                SessionKey: '207a11c0-12e3-4f7e-8033-f61b6883ffd8',
                RuleLogId: 'e445039d-f744-4da5-b4ad-b813d8a26f71',
                ProcessInstanceId: '3401927a-1f19-4f28-b7aa-b6e884a12f3e',
            }
        }).then(res=>{
            console.log(res);
            this.stepList = res.transitions;
        })
    },
    changeAll(e,item){
        item.Selected = e.mp.detail;
        item.ParticipantMember.forEach(v=>{
            v.Selected = item.Selected;
        })
    },
    onCloseAgree(){
        this.agreeShow = false;
    },
    changeItem(e,item,v){
        v.Selected = e.mp.detail;
        console.log(item.ParticipantMember.every(one=>one.Selected==true))
        for(let i=0;i<item.ParticipantMember.length;i++){
            if(item.ParticipantMember[i].Selected){
                item.Selected = true;
                break;
            }else {
                item.Selected = false;
            }
        }
    },
  }
};
</script>
<style lang="scss" scopod>
@import "../../../../static/css/public.scss";
@import "../../../../static/css/icon.css";
.wrap {
  .rowWrap {
    display: flex;
    justify-content: space-between;
    padding: 34rpx 33rpx;
    background: #fff;
    margin-top: 35rpx;
    .label {
      color: #666666;
      font-size: 32rpx;
      span {
        color: #ff6666;
      }
    }
    .name {
      span {
        font-size: 31rpx;
        color: #ababab;
      }
      span.active {
        color: #333333;
      }
    }
  }
  h3 {
    padding: 31rpx 33rpx;
    color: #3399ff;
    font-size: 26rpx;
  }
  .content {
    background: #fff;
    .row {
      display: flex;
      justify-content: space-between;
      padding: 34rpx 33rpx;
      background: #fff;
      border-bottom: 1rpx solid #e2e3e5;
      .label {
        color: #666666;
        font-size: 32rpx;
        span {
          color: #ff6666;
        }
      }
      .name {
        span {
          font-size: 31rpx;
          color: #ababab;
        }
        span.active {
          color: #333333;
        }
        input {
          font-size: 31rpx;
          text-align: right;
          margin-right: 20rpx;
        }
      }
    }
    .rowBottom {
      padding: 20rpx 33rpx;
      // border-top: 2rpx solid #eaebeb;
      p {
        font-size: 26rpx;
        color: #ababab;
        span {
          color: #3399ff;
          margin-left: 10rpx;
        }
      }
    }
  }
  .leaveComment {
    padding: 34rpx 33rpx;
    background: #fff;
    margin: 35rpx 0;
    p {
      font-size: 32rpx;
      color: #666666;
      span {
        color: #ff6666;
      }
    }
    .box {
      textarea {
        height: 100rpx;
        font-size: 31rpx;
        color: #333333;
      }
    }
  }
  .imgContent {
    background: #fff;
    margin-bottom: 20rpx;
    .head {
      font-size: 28rpx;
      display: flex;
      flex-direction: row;
      align-items: center;
      justify-content: space-between;
      padding: 20rpx 33rpx;
      color: #666666;
      .iconfont {
        color: #3399ff;
      }
    }
    .cont {
      padding: 20rpx;
      display: flex;
      flex-wrap: wrap;
      // justify-content: space-between;
      // flex-direction: row;
      .imgWrap {
        width: 100rpx;
        height: 100rpx;
        position: relative;
        margin-right: 20rpx;
        margin-top: 10rpx;
        img {
          width: 100%;
          height: 100%;
          vertical-align: middle;
          border-radius: 7rpx;
        }
        // .close{
        //     position: absolute;
        //     top: -10rpx;
        //     right: -10rpx;
        // }
        .close {
          display: inline-block;
          width: 33rpx;
          height: 33rpx;
          line-height: 33rpx;
          text-align: center;
          background: #9c9c9c;
          position: absolute;
          top: -10rpx;
          right: -10rpx;
          border-radius: 50%;
          opacity: 0.8;
        }
      }
    }
  }
  .agreeWrap {
    position: relative;
    background: #f2f2f2;
    .header {
      display: flex;
      padding: 30rpx;
      border-bottom: 1rpx solid #e2e4e3;
      width: 100%;
      background: #fff;
      // position: fixed;
      div:nth-child(2) {
        margin-left: 20rpx;
        .h3 {
          font-size: 28rpx;
          font-weight: bold;
        }
        p {
          font-size: 12px;
          color: #333333;
          margin-top: 10rpx;
          span {
            color: #666666;
          }
        }
      }
    }
    .cont {
      width: 100%;
      // position: absolute;
      // top: 70px;
      height: 400px;
      overflow: scroll;
      padding-bottom: 50px;
      h3 {
        background: #f2f2f2;
        font-size: 12px;
        color: #999999;
        padding: 20rpx;
        .checkbox {
          display: flex !important;
          align-items: center !important;
        }
        .slot {
          width: 28rpx;
          height: 28rpx;
          border: 2rpx solid #3399ff;
          border-radius: 50%;
          display: flex;
          justify-content: center;
          align-items: center;
          p {
            width: 13rpx;
            height: 13rpx;
            background: #3399ff;
            border-radius: 50%;
          }
        }
        .default {
          width: 28rpx;
          height: 28rpx;
          border: 2rpx solid #c8c9cc;
          border-radius: 50%;
        }
      }
      .box {
        padding: 20rpx;
        background: #fff;
        .row {
          padding: 20rpx 0;
          font-size: 28rpx;
          border-bottom: 1rpx solid #e2e4e3;
          color: #333333;
          display: flex;
          justify-content: space-between;
        }
        .rows {
          padding: 10rpx 0 0 0;
          font-size: 28rpx;
          color: #333333;
          display: flex;
          justify-content: center;
          .add {
            // text-align: center;
            color: #3399ff;
            span {
              margin-top: 10rpx;
              display: inline-block;
            }
          }
        }
      }
      .popupRow {
        .row {
          padding: 20rpx;
          color: #999999;
          font-size: 12px;
          background: #f2f2f2;
          display: flex;
          justify-content: space-between;
          p:nth-child(2) {
            span:nth-child(1) {
              display: inline-block;
              background: #3399ff;
              border-top-left-radius: 10rpx;
              border-bottom-left-radius: 10rpx;
              padding: 10rpx 20rpx;
              color: #fff;
            }
            span:nth-child(2) {
              display: inline-block;
              background: #fff;
              border-top-right-radius: 10rpx;
              border-bottom-right-radius: 10rpx;
              padding: 10rpx 20rpx;
              color: #3399ff;
              border: 1rpx solid #3399ff;
              box-sizing: border-box;
              border-left: none;
            }
          }
        }
        .text {
          padding: 20rpx;
          background: #fff;
          font-size: 12px;
          color: #333333;
        }
      }
      .textarea {
        padding: 20rpx;
        background: #fff;
        margin-top: 20rpx;
        p {
          font-size: 28rpx;
          font-weight: bold;
        }
        textarea {
          height: 80px;
        }
      }
    }
    .fot {
      width: 100%;
      position: fixed;
      bottom: 0;
      background: #fff;
      border-top: 1rpx solid #e2e3e5;
      .box {
        display: flex;
        justify-content: center;
        p {
          width: 50%;
          text-align: center;
          color: #3399ff;
          padding: 20rpx;
          border-right: 1rpx solid #f2f2f2;
        }
        p:last-child {
          background: #3399ff;
          color: #ffffff;
        }
      }
    }
  }
  .footer {
    width: 100%;
    position: fixed;
    bottom: 20rpx;
    background: #fff;
    .btn {
      padding: 20rpx;
    }
  }
}
</style>