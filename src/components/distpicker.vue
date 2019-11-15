<template>
  <div class="distpicker-address-wrapper">
    <div class="mask"></div>
    <div class="distpicker-address">
      <div class="address-header">
        <ul>
          <li class="active" @click.prevent.stop="transProvince">{{province}}</li>
          <li :class="city!==''?'active':''" @click.prevent.stop="transCity">{{city}}</li>
          <li :class="area!==''?'active':''">{{area}}</li>
        </ul>
      </div>
      <div class="address-container">
        <div class="initials" v-if="tag === 0">
          <div class="initials-item" v-for="(item, index) in Letter" :key="index" v-if="Initials[item]">
            <span class="initials-title">{{item}}</span>
            <ul>
              <li v-for="(ite, inx) in Initials[item]" :key="inx" @click.prevent.stop="getData(ite[idFieldName], ite[nameFieldName])">{{ite[nameFieldName]}}</li>
            </ul>
          </div>
        </div>
        <ul v-else-if="showData.length>0">
          <li class="" @click.prevent.stop="getData(item[idFieldName], item[nameFieldName])" v-for="(item, index) in showData" :key="index">{{item[nameFieldName]}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import {transInitials, Letter} from './address'

export default {
  name: 'distpicker',
  props: {
    showData: {
      type: Array,
      value: function () {
        return []
      }
    },
    idFieldName: {
      type: String,
      value: 'code'
    },
    nameFieldName: {
      type: String,
      value: 'msg'
    }
  },
  data () {
    return {
      province: '省',
      city: '',
      area: '',
      // 0 1 2 省市区
      tag: 0,
      Initials: {}, // 按首字母排序的省信息
      Letter: Letter
    }
  },
  created () {
    this.setInitials(this.showData)
  },
  methods: {
    // 设置以字母排序的省数据
    setInitials (provinceJson) {
      provinceJson.forEach(prov => {
        if (this.Initials[transInitials(prov[this.nameFieldName])]) {
          this.Initials[transInitials(prov[this.nameFieldName])].push(prov)
        } else {
          this.Initials[transInitials(prov[this.nameFieldName])] = [prov]
        }
      })
    },
    // 重新选择省
    transProvince () {
      this.tag = 0
      this.city = ''
      this.area = ''
      this.$emit('transProvince')
    },
    // 重新选择市
    transCity () {
      this.tag = 1
      this.area = ''
      this.$emit('transCity')
    },
    // 点击选择省/市/区
    getData (id, name) {
      if (this.tag === 0) {
        this.province = name
        this.onChangeProvince({
          id: id,
          value: name
        })
        this.tag = 1
      } else if (this.tag === 1) {
        this.city = name
        this.onChangeCity({
          id: id,
          value: name
        })
        this.tag = 2
      } else if (this.tag === 2) {
        this.area = name
        this.onChangeArea({
          id: id,
          value: name
        })
        this.tag = 0
      }
    },
    // 点击选择省
    onChangeProvince (a) {
      this.$emit('onChangeProvince', a)
    },
    // 点击选择市
    onChangeCity (a) {
      this.$emit('onChangeCity', a)
    },
    // 点击选择区
    onChangeArea (a) {
      this.$emit('onChangeArea', a)
    }
  }
}
</script>

<style scoped>
  ul li{
    list-style: none;
  }
  .distpicker-address-wrapper{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1002;
  }
  .mask{
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
  }
  .distpicker-address{
    position: absolute;
    width: 100%;
    bottom: 0;
    color: rgb(153, 153, 153);
  }
  .distpicker-address-wrapper .address-header{
    background-color: #fff;
  }
  .distpicker-address-wrapper ul {
    margin: 0;
    padding: 0;
  }
  .distpicker-address-wrapper .address-header ul{
    display: flex;
    justify-content: space-around;
    align-items: stretch;
  }
  .distpicker-address-wrapper .address-header ul li {
    padding: 8px 0.3rem;
    color: #000;
  }
  .distpicker-address-wrapper .address-header .active {
    border-bottom: rgb(255, 92, 31) solid 3px;
    color: rgb(255, 92, 31);
  }
  .distpicker-address-wrapper .address-container {
    background-color: #fff;
  }
  .distpicker-address-wrapper .address-container {
    max-height: 300px;
    overflow: scroll;
  }
  .distpicker-address-wrapper .address-container ul {
    height: 100%;
    overflow: auto;
  }
  .initials-title{
    padding-left: 10px;
  }
  .distpicker-address-wrapper .address-container ul li {
    padding: 8px 10px;
    border-top: 1px solid #f6f6f6;
  }
</style>
