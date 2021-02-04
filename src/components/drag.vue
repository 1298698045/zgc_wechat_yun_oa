<template>
  <div>
    <div style="width: 90%; height: 40px; line-height: 40px; margin: auto">
      <div style="float: left; font-size: 15px">我的应用</div>
      <div style="float: right; font-size: 15px" @click="edit">
        {{ status == false ? "编辑" : "完成" }}
      </div>
    </div>
    <div class="funslist">
      <movable-area style="width: 100%; margin: auto; height: 180px">
        <div v-for="(item, index) in choose_list" :key="index">
          <div
            v-if="item.status == true"
            class="application"
            :data-index="index"
            @longpress="longtap"
            @touchstart="touchs(index)"
            @touchend="touchend"
            @touchmove="touchm"
          >
            <img class="fi-img" :src="item.src" />
            <view class="fi-text">{{ item.name }}</view>
            <view
              class="fi-badge"
              @click="alldelfun(item.id, index)"
              v-if="status == true"
              >-</view
            >
          </div>
        </div>
        <div style="clear: both"></div>
        <movable-view
          animation
          direction="all"
          damping="5000"
          :x="x"
          :y="y"
          friction="1"
          :disabled="disabled"
        >
          <view :hidden="hidden">
            <div class="application1">
              <img class="fi-img" :src="list.src" />
              <view class="fi-text">{{ list.name }}</view>
            </div>
          </view>
        </movable-view>
      </movable-area>
    </div>
    <div style="margin-top: 20px">
      <h2 class="title">全部应用</h2>
      <div class="funslist">
        <div v-for="(item, index) in all_list" :key="index">
          <div class="application1">
            <img class="fi-img" :src="item.src" />
            <view class="fi-text">{{ item.name }}</view>
            <view
              class="fi-badge"
              v-if="item.status == true && status == true"
              @click="alldelfun(item.id, index)"
              >-</view
            >
            <view
              class="fi-badge"
              v-if="item.status == false && status == true"
              style="background: #1892e0"
              @click="alladdfun(item.id, index)"
              >+</view
            >
          </div>
        </div>
        <div style="clear: both"></div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Drag",
  data() {
    return {
      hidden: true,
      flag: false,
      x: 0,
      y: 0,
      disabled: true,
      elements: [],
      all_list: [
        {
          id: 1,
          name: "我的课程",
          src: "../../static/image/new_11.png",
          url: "../studentTimetab/main",
          status: true,
          left:40,
          right:100,
          top:40,
          bottom:150,
        },
        {
          id: 2,
          name: "点击报名",
          src: "../../static/image/new_13.png",
          url: "../student/main",
          status: true,
          left:100,
          right:160,
          top:40,
          bottom:150,
        },
        {
          id: 3,
          name: "学生菜谱",
          src: "../../static/image/new_14.png",
          url: "../studentMenu/main",
          status: true,
          left:160,
          right:220,
          top:40,
          bottom:150,
        },
        {
          id: 4,
          name: "请假申请",
          src: "../../static/image/new_15.png",
          url: "../studentLeave/main",
          status: true,
          left:240,
          right:280,
          top:100,
          bottom:160,
        },
      ],
      choose_list: [
        {
          id: 1,
          name: "我的课程",
          src: "../../static/image/new_11.png",
          url: "../studentTimetab/main",
          status: true,
        },
        {
          id: 2,
          name: "点击报名",
          src: "../../static/image/new_13.png",
          url: "../student/main",
          status: true,
        },
        {
          id: 3,
          name: "学生菜谱",
          src: "../../static/image/new_14.png",
          url: "../studentMenu/main",
          status: true,
        },
        {
          id: 4,
          name: "请假申请",
          src: "../../static/image/new_15.png",
          url: "../studentLeave/main",
          status: true,
        },
      ],
      beginIndex: null,
      list: {},
      status: false,
    };
  },
  onLoad(){

  },
  methods: {
    //编辑
    edit() {
      this.status = !this.status;
    },
    //删减
    alldelfun(id, index) {
      this.all_list[index].status = false;
      this.choose_list[index].status = false;
    },
    alladdfun(id, index) {
      let num = 0;
      let that = this;
      this.all_list.forEach((e) => {
        if (e.status == true) {
          num++;
        }
      });
      if (num == 7) {
        this.$index.showToast("应用已达最高数量,请先移除部分再添加");
      } else {
        that.$set(that.all_list[index], "status", true);
        that.$set(that.choose_list[index], "status", true);
      }
    },
    //长按
    longtap(e) {
      if (this.status == true) {
        this.x = e.currentTarget.offsetLeft;
        this.y = e.currentTarget.offsetTop;
        this.hidden = false;
        this.flag = true;
      }
    },
    //触摸开始
    touchs(index) {
      if (this.status == true) {
        this.beginIndex = index;
        this.list = this.choose_list[index];
      }
    },
    //触摸结束
    touchend(e) {
      let that = this;
      if (!that.flag) {
        return;
      }
      const endx = e.mp.changedTouches[0].pageX;
      const endy = e.mp.changedTouches[0].pageY;
      for (var j = 0; j < that.all_list.length; j++) {
        const item = that.all_list[j];
        if (
          endx > item.left &&
          endx < item.right &&
          endy > item.top &&
          endy < item.bottom
        ) {
            
          var endIndex = item.id;
          endIndex = endIndex-1
          const beginIndex = that.beginIndex;
          console.log(endIndex,'endIndex')
            console.log(beginIndex,'beginIndex')
          //向后移动
          if (beginIndex < endIndex) {
            console.log("向后移");
            let tem = that.choose_list[beginIndex];
            for (let i = beginIndex; i < endIndex; i++) {
              that.choose_list[i] = that.choose_list[i + 1];
            }
            that.choose_list[endIndex] = tem;
          }
          //向前移动
          if (beginIndex > endIndex) {

            console.log("向前移");
            let tem = that.choose_list[beginIndex];
            for (let i = beginIndex; i > endIndex; i--) {
              that.choose_list[i] = that.choose_list[i - 1];
            }
            that.choose_list[endIndex] = tem;
          }
        }
      }
      that.hidden = true;
      that.flag = false;
    },
    //滑动
    touchm(e) {
      if (this.flag && this.status == true) {
        const x = e.mp.touches[0].pageX;
        const y = e.mp.touches[0].pageY;
        this.x = x - 65;
        this.y = y - 45;
      }
    },
  },
  mounted() {
    var query = wx.createSelectorQuery();
    var nodesRef = query.selectAll(".application");
    nodesRef
      .fields(
        {
          dataset: true,
          rect: true,
        },
        (result) => {
          this.elements = result;
          console.log(this.elements);
        }
      )
      .exec();
  },
};
</script>
<style>
.title {
  width: 90%;
  margin: 10px auto 0;
  font-size: 15px;
}
.funslist {
  width: 90%;
  margin: 0 auto;
  margin-top: 10px;
}
.funslist .fi-img {
  display: block;
  width: 40%;
  height: 30px;
  margin: 10px auto 5px;
}
.application,
.application1 {
  width: 75px;
  height: 75px;
  margin-left: 5px;
  margin-right: 5px;
  float: left;
  background: #eee;
  margin-bottom: 10px;
  border-radius: 5px;
  padding-bottom: 5px;
  position: relative;
}
.fi-text {
  width: 100%;
  height: 20px;
  line-height: 20px;
  font-size: 13px;
  text-align: center;
}
.fi-badge {
  position: absolute;
  width: 15px;
  height: 15px;
  border-radius: 50%;
  color: #fff;
  background: #e34f57;
  top: 5px;
  right: 5px;
  color: #fff;
  text-align: center;
  line-height: 12px;
  font-size: 14px;
}
</style>