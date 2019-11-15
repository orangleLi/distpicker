<template>
  <div id="app">
    <div class="box">
      <div class="btn" @click="clickBtn">Click me</div>
      <span>{{address}}</span>
    </div>
    <distpicker v-if="show"
                :showData="showData"
                :idFieldName="idFieldName"
                :nameFieldName="nameFieldName"
                @onChangeProvince="onChangeProvince"
                @onChangeCity="onChangeCity"
                @onChangeArea="onChangeArea"
                @transProvince="transProvince"
                @transCity="transCity"></distpicker>
  </div>
</template>

<script>
import {provinceJson, cityJson, areaJson} from './components/address.js'
import Distpicker from './components/distpicker'
export default {
  name: 'App',
  components: {Distpicker},
  data () {
    return {
      show: false,
      showData: [],
      provinceArr: [],
      cityArr: [],
      idFieldName: 'code',
      nameFieldName: 'msg',
      province: '',
      city: '',
      area: ''
    }
  },
  created () {
    this.getProvinceList()
  },
  methods: {
    getProvinceList () {
      let that = this
      // 获取数据 provinceJson
      that.provinceArr = provinceJson
      that.$set(that, 'showData', provinceJson)
      // this.$get('api/GetProvinceList', {}).then(res => {
      //   if (res.resultCode === '1') {
      //     that.provinceArr = res.resultValue
      //     that.showData = res.resultValue
      //   }
      // })
    },
    getCityArr (provinceId) {
      let that = this
      that.cityArr = cityJson[provinceId]
      that.$set(that, 'showData', cityJson[provinceId])
      // let paramData = {
      //   param: {
      //     provinceId: provinceId
      //   }
      // }
      // this.$get('api/GetCityList', paramData).then(res => {
      //   if (res.resultCode === '1') {
      //     that.cityArr = res.resultValue
      //     that.$set(that, 'showData', res.resultValue)
      //   }
      // })
    },
    getAreaArr (cityId) {
      let that = this
      that.$set(that, 'showData', areaJson[cityId])
      // let paramData = {
      //   param: {
      //     cityId: cityId
      //   }
      // }
      // this.$get('api/GetDistrictList', paramData).then(res => {
      //   if (res.resultCode === '1') {
      //     that.$set(that, 'showData', res.resultValue)
      //   }
      // })
    },
    transProvince () {
      this.$set(this, 'showData', this.provinceArr)
    },
    transCity () {
      this.$set(this, 'showData', this.cityArr)
    },
    clickBtn () {
      this.show = !this.show
      if (this.show) {
        this.province = ''
        this.city = ''
        this.area = ''
        this.getProvinceList()
      }
    },
    onChangeProvince: function (a) {
      this.getCityArr(a.id)
      this.province = a.value
    },
    onChangeCity: function (a) {
      this.getAreaArr(a.id)
      this.city = a.value
    },
    onChangeArea: function (a) {
      this.area = a.value
      this.clickBtn()
    }
  },
  computed: {
    address () {
      return this.province + this.city + this.area
    }
  }
}
</script>

<style scoped>
  .btn {
    width: 80px;
    font-size: 14px;
    padding: .3em .8em;
    border: 1px solid rgba(0, 0, 0, .1);
    background: #58a linear-gradient(hsla(0, 10%, 100%, .2), transparent);
    border-radius: .2em;
    box-shadow: 0 .05em .25em rgba(0, 0, 0, 0.5);
    color: white;
    text-shadow: 0 -.05em .05em rgba(0, 0, 0, 0.5);
    text-align: center;
    transform: translate(0px, 0px);
  }
  .btn:active{
    transform: translate(0.5px, 0.5px);
    opacity: 0.8;
  }
</style>
