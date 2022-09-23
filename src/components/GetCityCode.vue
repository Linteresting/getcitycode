<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div>
      <span>city:</span>
      <input v-model="city">
    </div>
    <div>
      <span>cityCode:</span>
      <input v-model="cityCode">
    </div>
    <div>
      <button @click="getCityCode">GET!</button>
    </div>
    <div>
      <span>{{ getMsg }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GetCityCode',
  props: {
    msg: String
  },
  data() {
    return {
      city: '',
      cityCode: '',
      initFlag: '',
      getMsg: '',
      codeToNameMap: {},
      nameToCodeMap: {}
    }
  },
  methods: {
    getCityCode() {
      console.log(`getCityCode click! ${this.city} ${this.cityCode}`)
      if (this.initFlag == 0) {
        console.log(`getCityCode click! `)
        const cityList = require('../assets/citylist.json')
        console.log(cityList)
        this.codeToNameMap = new Map
        this.nameToCodeMap = new Map
        for(var item of cityList) {
          this.codeToNameMap.set(item.adCode, item.name)
          this.nameToCodeMap.set(item.name, item.adCode)
        }
        this.initFlag = 1
      }

      if (this.city == '' && this.cityCode == '') {
        this.getMsg = '请输入城市或者城市码'
      }
      if (this.city != '') {
        if (this.city.search('市') == -1) {
          this.city = this.city + '市'
        }
        let resultCode = this.nameToCodeMap.get(this.city)
        this.getMsg = `城市码为${resultCode}`
      }
      if (this.cityCode != '') {
        let resultCity = this.codeToNameMap.get(this.cityCode)
        this.getMsg = `城市为${resultCity}`
      }

    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div {
  margin: 20px 0 0;
}
</style>
