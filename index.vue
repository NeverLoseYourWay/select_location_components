<template>
  <div>
    <h1>地址联动选择</h1>
    <div style="width: 300px">
      <cascader
        v-model="locationForm"
        v-clickoutside="handleClose1"
        icon="iconPrimary"
        ref="cascaderRef"
        :placeArr1="placeArr1"
        :placeArr2="placeArr2"
        :placeArr3="placeArr3"
        :placeArr4="placeArr4"
      ></cascader>
    </div>
  </div>
</template>

<script>
import cascader from './cascader'
import clickoutside from 'element-ui/src/utils/clickoutside' //这里借助elementui的

export default {
  components: {
    cascader
  },
  directives: {
    clickoutside
  },
  data() {
    return {
      locationForm: {
        Place: [],
        PlaceText: ''
      },
      placeArr1: [],
      placeArr2: [],
      placeArr3: [],
      placeArr4: []
    }
  },
  methods: {
    handleClose1() {
      this.$refs.cascaderRef.getFocusResult1 = true
    }
  }
}
</script>

<style lang="scss" scoped></style>
