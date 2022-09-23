<template>
  <div class="container">
    <div class="msg">
      <h1>{{ msg }}</h1>
    </div>

    <div class="group">      
      <input v-model="city" type="text" required>
      <span class="highlight"></span>
      <span class="bar"></span>
      <label>城市</label>
    </div>

    <div class="group">      
      <input v-model="cityCode" type="text" required>
      <span class="highlight"></span>
      <span class="bar"></span>
      <label>cityCode</label>
    </div>

    <div>
      <button @click="getCityCode">GET!</button>
    </div>
    <div class="getMsg">
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
        console.log(`getCityCode init`)
        const cityList = require('../assets/citylist.json')
        // console.log(cityList)
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
        return
      }
      if (this.city != '') {
        if (this.city.search('市') == -1) {
          this.city = this.city + '市'
        }
        let resultCode = this.nameToCodeMap.get(this.city)
        this.getMsg = `城市码为${resultCode}`
        if (navigator.clipboard) {
          navigator.clipboard.writeText(resultCode);
          this.getMsg = `城市码 ${resultCode} 已经复制到剪切板，直接去粘贴吧！`
        }
        return
      }
      if (this.cityCode != '') {
        let resultCity = this.codeToNameMap.get(this.cityCode)
        this.getMsg = `城市为${resultCity}`
        if (navigator.clipboard) {
          navigator.clipboard.writeText(resultCity);
          this.getMsg = `城市 ${resultCity} 已经复制到剪切板，直接去粘贴吧！`
        }
      }
    },
    onCreate() {
      console.log(`page init`)
      if (this.initFlag == 0) {
        console.log(`getCityCode init`)
        const cityList = require('../assets/citylist.json')
        // console.log(cityList)
        this.codeToNameMap = new Map
        this.nameToCodeMap = new Map
        for(var item of cityList) {
          this.codeToNameMap.set(item.adCode, item.name)
          this.nameToCodeMap.set(item.name, item.adCode)
        }
        this.initFlag = 1
      }
      let word = location.pathname.slice(1)
      let result = ''
      if (word != '') {
        console.log(word)
        if (isNaN(Number(word, 10))) {
          // 非数字
          word = decodeURI(word)
          if (word.search('市') == -1) {
            word = word + '市'
          }
          result = this.nameToCodeMap.get(word)
          this.city = word
          this.cityCode = result
          if (navigator.clipboard) {
            navigator.clipboard.writeText(result);
            this.getMsg = `城市码 ${result} 已经复制到剪切板，直接去粘贴吧！`
          }
        } else {
          result = this.codeToNameMap.get(word)
          this.city = result
          this.cityCode = word
          if (navigator.clipboard) {
            navigator.clipboard.writeText(result);
            this.getMsg = `城市 ${result} 已经复制到剪切板，直接去粘贴吧！`
          }
        }
        // alert(result)
      }
    }
  },
  mounted:function () {
    this.onCreate()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

* {
  box-sizing:border-box;
}

button {
  display: inline-block;
  padding: 15px 25px;
  font-size: 18px;
  cursor: pointer;
  text-align: center;   
  text-decoration: none;
  outline: none;
  color: #fff;
  background-color: #5264AE;
  border: none;
  border-radius: 15px;
  box-shadow: 0 4px #999;
}
button:hover {background-color: #3e8e41}

button:active {
  background-color: #3e8e41;
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}

.container { 
  font-family:'Roboto';
  width:600px; 
  margin:30px auto 0; 
  display:block; 
  background:#FFF;
  padding:10px 50px 50px;
}
h2 { 
  text-align:center; 
  margin-bottom:50px; 
}
h2 small { 
  font-weight:normal; 
  color:#888; 
  display:block; 
}

/* form starting stylings ------------------------------- */
.group { 
  position:relative; 
  margin-bottom:45px; 
}
input {
  font-size:18px;
  padding:10px 10px 10px 5px;
  display:block;
  width:300px;
  border:none;
  border-bottom:1px solid #757575;
}
input:focus { outline:none; }

/* LABEL ======================================= */
label {
  color:#999; 
  font-size:18px;
  font-weight:normal;
  position:absolute;
  pointer-events:none;
  left:5px;
  top:10px;
  transition:0.2s ease all; 
  -moz-transition:0.2s ease all; 
  -webkit-transition:0.2s ease all;
}

/* active state */
input:focus ~ label, input:valid ~ label 		{
  top:-20px;
  font-size:14px;
  color:#5264AE;
}

/* BOTTOM BARS ================================= */
.bar 	{ position:relative; display:block; width:300px; }
.bar:before, .bar:after 	{
  content:'';
  height:2px; 
  width:0;
  bottom:1px; 
  position:absolute;
  background:#5264AE; 
  transition:0.2s ease all; 
  -moz-transition:0.2s ease all; 
  -webkit-transition:0.2s ease all;
}
.bar:before {
  left:50%;
}
.bar:after {
  right:50%; 
}

/* active state */
input:focus ~ .bar:before, input:focus ~ .bar:after {
  width:50%;
}

/* HIGHLIGHTER ================================== */
.highlight {
  position:absolute;
  height:60%; 
  width:100px; 
  top:25%; 
  left:0;
  pointer-events:none;
  opacity:0.5;
}

/* active state */
input:focus ~ .highlight {
  -webkit-animation:inputHighlighter 0.3s ease;
  -moz-animation:inputHighlighter 0.3s ease;
  animation:inputHighlighter 0.3s ease;
}

/* ANIMATIONS ================ */
@-webkit-keyframes inputHighlighter {
	from { background:#5264AE; }
  to 	{ width:0; background:transparent; }
}
@-moz-keyframes inputHighlighter {
	from { background:#5264AE; }
  to 	{ width:0; background:transparent; }
}
@keyframes inputHighlighter {
	from { background:#5264AE; }
  to 	{ width:0; background:transparent; }
}

.getMsg {
  margin-top: 50px;
}

.msg {
  margin-bottom: 50px;
}

.group {
  margin-left: 100px;
}

</style>
