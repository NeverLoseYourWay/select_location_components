<template>
  <div>
    <div @click="getFocusClick1" ref="zone" class="input-area">
      <el-input
        placeholder="省/市/县区/乡镇街"
        :value="locationForm.PlaceText"
        class="mt-8 trailer_start"
        :class="icon"
        ref="zoneInput"
        :disabled="disabledFlag"
        style="height: 46px"
        prefix-icon="el-icon-location"
      >
        <i
          slot="suffix"
          class="el-input__icon el-icon-arrow-down tt"
          :class="getFocusResult1 ? 'tu' : 'td'"
        ></i>
      </el-input>
    </div>
    <div class="trailer_start_PlaceText">
      <div
        v-if="!getFocusResult1"
        class="trailer_dropdown el-select-dropdown el-popper"
        x-placement="bottom-start"
        id="trailer_dropdown_1"
      >
        <section class="picker_content">
          <div class="tabs">
            <div
              class="tab"
              v-for="(item, index) in tabList1"
              :key="item.ID"
              :class="onIndex1 === item.ID ? 'on' : ''"
              @click="openIndex(index)"
            >
              {{ item.Name }}
            </div>
            <i class="el-icon-close crp close_icon" @click="initOptStart"></i>
          </div>
          <div class="panel">
            <div class="list" :class="onIndex1 === 0 ? 'd-block' : 'd-none'">
              <div v-if="tabList1[0].arr.length === 0">
                <div class="failed-info">
                  <i class="el-icon-info"></i>
                  获取省份数据失败,请点击
                  <span
                    style="color: red; cursor: pointer"
                    @click="getProvinceInit"
                  >
                    重试
                  </span>
                </div>
              </div>
              <a
                v-else
                :title="item.Name"
                class="a_div"
                v-for="(item, index) in tabList1[0].arr"
                :key="index"
                @click="choosePlace(item)"
              >
                {{ item.Name }}
              </a>
            </div>
            <div class="list" :class="onIndex1 === 1 ? 'd-block' : 'd-none'">
              <div v-if="loading">
                <i class="el-icon-loading"></i>
                加载数据中...
              </div>
              <div v-else-if="tabList1[1].arr.length === 0">
                <div class="failed-info">
                  <i class="el-icon-info"></i>
                  请先选择省份!
                </div>
              </div>
              <div v-else>
                <a
                  :title="item.Name"
                  class="a_div"
                  v-for="(item, index) in tabList1[1].arr"
                  :key="index"
                  @click="choosePlace(item)"
                >
                  {{ item.Name }}
                </a>
              </div>
            </div>
            <div class="list" :class="onIndex1 === 2 ? 'd-block' : 'd-none'">
              <div v-if="loading">
                <i class="el-icon-loading"></i>
                加载数据中...
              </div>
              <div v-else-if="tabList1[2].arr.length === 0">
                <div class="failed-info">
                  <i class="el-icon-info"></i>
                  请先选择城市!
                </div>
              </div>
              <div>
                <a
                  :title="item.Name"
                  class="a_div"
                  v-for="(item, index) in tabList1[2].arr"
                  :key="index"
                  @click="choosePlace(item)"
                >
                  {{ item.Name }}
                </a>
              </div>
            </div>
            <div class="list" :class="onIndex1 === 3 ? 'd-block' : 'd-none'">
              <div v-if="loading">
                <i class="el-icon-loading"></i>
                加载数据中...
              </div>
              <div v-else-if="tabList1[3].arr.length === 0">
                <div class="failed-info">
                  <i class="el-icon-info"></i>
                  请先选择县区!
                </div>
              </div>
              <div>
                <a
                  :title="item.Name"
                  class="a_div"
                  v-for="(item, index) in tabList1[3].arr"
                  :key="index"
                  @click="choosePlace(item)"
                >
                  {{ item.Name }}
                </a>
              </div>
            </div>
          </div>
        </section>
      </div>
    </div>
  </div>
</template>
<script>
import global from '@/assets/js/global'
export default {
  model: {
    prop: 'locationForm'
    // event: "change"
  },
  props: {
    locationForm: Object,
    icon: String,
    placeArr1: Array,
    placeArr2: Array,
    placeArr3: Array,
    placeArr4: Array,
    disabledFlag: {
      type: Boolean,
      require: false,
      default: false
    },
    onIndex: {
      type: Number,
      require: false,
      default: 0
    }
  },
  watch: {
    placeArr1(newVal) {
      this.tabList1[0].arr = newVal
    },
    placeArr2(newVal) {
      this.tabList1[1].arr = newVal
    },
    placeArr3(newVal) {
      this.tabList1[2].arr = newVal
    },
    placeArr4(newVal) {
      this.tabList1[3].arr = newVal
    },
    onIndex(newVal) {
      this.onIndex1 = Number(newVal)
      console.log('xxxrrr', newVal)
    },
    disabledFlag(newVal) {
      if (newVal) {
        this.getFocusResult1 = newVal
      }
    }
  },
  data() {
    return {
      loading: false,
      onIndex1: 0,
      tabList1: [
        { Name: '省份', ID: 0, arr: [] },
        { Name: '城市', ID: 1, arr: [] },
        { Name: '县区', ID: 2, arr: [] },
        { Name: '街道', ID: 3, arr: [] }
      ],
      getFocusResult1: true
    }
  },
  methods: {
    getFocusClick1() {
      if (this.disabledFlag) return
      this.getFocusResult1 = !this.getFocusResult1
    },
    openIndex(val) {
      this.onIndex1 = val
    },
    initOptStart() {
      //关闭
      this.getFocusResult1 = true
    },
    getPlace(val) {
      this.loading = true
      //进行请求window.$axios ,val可作为参数,下面是虚拟数据
      console.log('data', val)
      this.tabList1[this.onIndex1].arr = [
        {
          Name: '广东省',
          ID: '0001'
        }
      ]
      this.loading = false
    },
    getProvinceInit() {
      //获取省份接口 window.$axios...下面是虚拟数据
      this.tabList1[0].arr = [
        {
          Name: '广东省',
          ID: '0001'
        }
      ]
    },
    choosePlace(item) {
      let originTalist = this.$options.data().tabList1
      this.locationForm.Place[this.onIndex1] = JSON.parse(JSON.stringify(item))
      this.tabList1[this.onIndex1].Name = item.Name
      this.onIndex1 = this.onIndex1 + 1
      if (this.onIndex1 > 3) {
        this.getFocusResult1 = true
        this.locationForm.PlaceText = ''
        this.locationForm.Place.map((item) => {
          this.locationForm.PlaceText += item.Name
        })
        this.onIndex1 = 3
        return false
      }
      if (this.onIndex1 === 1) {
        this.tabList1 = this.tabList1.slice(0, 1).concat(originTalist.slice(1))
        let id
        if (global.isFilled(this.locationForm.Place[0])) {
          id = this.locationForm.Place[0].ID
          this.getPlace(id)
        }
      } else if (this.onIndex1 === 2) {
        this.tabList1 = this.tabList1.slice(0, 2).concat(originTalist.slice(2))
        let id
        if (global.isFilled(this.locationForm.Place[1])) {
          id = this.locationForm.Place[1].ID
          this.getPlace(id)
        }
      } else if (this.onIndex1 === 3) {
        this.tabList1 = this.tabList1.slice(0, 3).concat(originTalist.slice(3))
        let id
        if (global.isFilled(this.locationForm.Place[2])) {
          id = this.locationForm.Place[2].ID
          this.getPlace(id)
        }
      }
    }
  }
}
</script>
<style lang="scss" scoped>
$primary: #f06e32;
$mlColor: #999999;
.tt {
  transition: transform 0.3s ease;
}
.td {
  transform: rotate(-180deg);
}
.tu {
  transform: rotate(0deg);
}
.d-none {
  display: none;
}
.d-block {
  display: block;
}
.input-area {
  // width: 26.6%; //276
  /deep/.el-input__inner {
    height: 46px !important;
    background: #f2f2f2;
    border: none;
  }
  /deep/.el-input__prefix {
    color: #2ba5ee;
    top: 2px;
  }
  .iconPrimary {
    /deep/.el-input__prefix {
      color: #f06e32;
      top: 2px;
    }
  }
}
.trainer_none {
  display: none !important;
}
.trailer_dropdown {
  position: absolute;
  // height: 50px;
  min-width: 280px;
  transform-origin: center bottom;
  z-index: 2025;
  // display: none;
  .picker_content {
    max-width: 400px;
    max-height: 400px;
    width: 400px;
    font-size: 14px;
    background: #fff;
    .tabs {
      box-sizing: content-box;
      justify-content: space-between;
      display: flex;
      height: 38px;
      line-height: 38px;
      border-bottom: 1px solid #e9e9e9;
      background: #fcfcfc;
      .tab {
        display: inline-block;
        box-sizing: content-box;
        height: 100%;
        padding: 0 11px;
        border: 1px solid transparent;
        border-width: 0 1px;
        font-size: 14px;
        width: 4.5em;
        text-align: center;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        cursor: pointer;
      }
      .on {
        margin-bottom: -1px;
        border: 1px solid #e9e9e9;
        border-top: none;
        border-bottom-color: #fff;
        color: $primary;
        background: #fff;
      }
      .close_icon {
        width: 40px;
        height: 38px;
        font-size: 20px;
        color: #999;
        line-height: 38px;
        text-align: center;
        cursor: pointer;
        transform: rotate(0);
        transition: transform 0.2s;
        &:hover {
          color: $primary;
          transform: rotate(90deg);
        }
      }
    }
    .panel {
      padding: 10px;
      height: 180px;
      width: 400px;
      overflow: auto;
      box-sizing: border-box;
      text-align: left;
      .list {
        //加载异常
        .failed-info {
          color: #bdbdbd;
          margin-top: 50px;
          text-align: center;
        }
        .a_div {
          box-sizing: content-box;
          margin: 0 10px;
          width: 100px;
          display: inline-block;
          height: 32px;
          line-height: 32px;
          white-space: nowrap;
          overflow: hidden;
          text-overflow: ellipsis;
          color: $mlColor;
          // font-size: 14px;
        }
        a {
          text-decoration: none;
          outline: 0;
          cursor: pointer;
          transition: color 0.2s ease;
        }
        a:hover {
          color: $primary;
        }
      }
    }
  }
}
</style>
