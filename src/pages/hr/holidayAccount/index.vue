<template>
  <div class="wrap">
    <view class="table">
      <view class="tr bg-w">
        <view class="th w height lh">代码</view>
        <view class="th w height lh">名称</view>
        <view class="th f height row">
          <p>休假情况</p>
          <div class="text">
            <p>总数</p>
            <p>已使用</p>
            <p>未使用</p>
          </div>
        </view>
      </view>
      <block v-for="(item,index) in listData" :key="index">
      <view class="tr bg-g" v-if="''">
        <view class="td">{{item.code}}</view>
        <view class="td">{{item.text}}</view>
        <view class="td">{{item.type}}</view>
      </view>
      <view class="tr bg-g" v-else>
        <view class="td">{{item.code}}</view>
        <view class="td">{{item.text}}</view>
        <view class="td">{{item.type}}</view>
        <view class="td">{{item.type}}</view>
        <view class="td">{{item.type}}</view>
      </view>
      </block>
    </view>
  </div>
</template>
<script>
export default {
  data(){
    return {
      listData:[
        {"code":"01","text":"text1","type":"type1"},
        {"code":"02","text":"text2","type":"type2"},
        {"code":"03","text":"text3","type":"type3"},
        {"code":"04","text":"text4","type":"type4"},
        {"code":"05","text":"text5","type":"type5"},
        {"code":"06","text":"text6","type":"type6"},
        {"code":"07","text":"text7","type":"type7"}
      ]
    }
  }
}
</script>
<style lang="scss">
  .table {
 border: 0px solid darkgray;
}
.tr {
 display: flex;
 width: 100%;
 justify-content: center;
//  height: 100rpx;
 align-items: center;
 background: #fff;
}
.td {
 width:40%;
 line-height: 100rpx;
 justify-content: center;
 text-align: center;
}
.bg-w{
 background: snow;
}
.bg-g{
 background: #E6F3F9;
}
.th {
 width: 40%;
 justify-content: center;
 background: #3399ff;
 color: #fff;
 text-align: center;
//  display: flex;
//  height: 100rpx;
 align-items: center;
}
.w{
  width: 75px;
}
.f{
  flex: 1;
}
.height{
  height: 100rpx;
}
.lh{
  line-height: 100rpx;
}
.row{
  .text{
    display: flex;
    font-size: 28rpx;
    p{
      width: 75px;
      text-align: center;
    }
  }
}
</style>