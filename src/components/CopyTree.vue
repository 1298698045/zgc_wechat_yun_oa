<template>
  <ul class="ul-wrapper">                                    <!-- 包裹层-->
    <li v-for="item in list" :key="item.key">                <!-- 遍历-->
      <div>
          <i-icon type="unfold" size="20" color="#999999"  v-show="item.children" @click="changeShow" />
        <!-- <checkbox @change="clickbox" :value='item.key' :checked="isChecked(item.key)" :disabled="item.disabled">{{item.title}}</checkbox> -->
        <input
          type="checkbox"
          :name="item.key"
          @click="clickbox"
          :checked="isChecked(item.key)"
          :disabled="item.disabled"
        >
        <div class="checkbox-title">{{item.title}}</div>      <!-- CheckBox内容-->
      </div>
    </li>
  </ul>
</template>

<script type='text/ecmascript-6'>
export default {
  name: 'tree',
  props: {
    list: { // 所有树节点
      type: Array,
      default: () => {
        return []
      }
    }
  },
  data () {
    return {
      showChildren: true // 是否展开根目录
    }
  },
  computed: { // computed属性计算展开图标
    imgUrl () {
      return this.showChildren ? 'down.png' : 'right.png'
    }
  },
  methods: {
    clickbox (e) { // 点击CheckBox时需要加入或删除已选中this.$global.nodes的节点数组中
      console.log(e,this.list,this.$global.nodes);
      const checked = e.target.checked
      const key = e.target.name
      const nodes = this.$global.nodes // this.$global.nodes是全局变量，便于递归组件记录选中节点
      if (checked) {
        if (!nodes.includes(key)) {
          this.$global.nodes.push(key)
        }
      } else {
        this.$global.nodes = nodes.filter((item) => {
          return key !== item
        })
      }
    },
    changeShow () { // 点击是否展开根目录，当前状态取反即可
      this.showChildren = !this.showChildren
    },
    isChecked (key) { // 查看是否已经存在于选中节点中
      return this.$global.nodes.includes(key)
    }
  }
}
</script>

<style lang="scss">
.ul-wrapper {
  margin:10px 30px 0;
    .checkbox-title, .icon{
        display:inline-block;
        vertical-align:middle;
        .icon{
            margin-left:20px;
            height:18px;
            width:18px;
        }
        input[type="checkbox"]{
            position :relative;
            display :inline-block;
            vertical-align: middle;
            padding :0;
            margin-right: 5px;
            height :18px;
            width :18px;
            border :1px solid #ccc;
            border-radius: 3px;
        }
        input[type="checkbox"]:checked::before{
            position :absolute;
            top :0;
            left :0;
            padding-left: 2px;
            content: "\2713";
            height :15px;
            width :13px;
            font-size: 12px;
            font-weight: bold;
            background: #1296db;
            color :#fff;
            border-radius: 3px;
            border :0;
        }
        input[type="checkbox"]:disabled::before{
            position :absolute;
            top :0;
            left :0;
            padding-left: 2px;
            content: "";
            height :15px;
            width :13px;
            font-size: 12px;
            font-weight: bold;
            background :#ccc;
            color :#fff;
            border-radius: 3px;
            border :0;
        }
    }
}
</style>