<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1>{{msg}}</h1>
    <button @click="formJava">从java来</button>
    <template v-if="imgSrc">
      <img :src="imgSrc" style="width: 200px;height: 200px"/>
    </template>
  </div>
</template>

<script>
import  './config/bridge'

export default {
  name: 'App',
  data () {
    return {
      msg: 'safsd',
      imgSrc:"",
    }
  },
  components: {
  },
  beforeCreate() {
      console.log("子组件beforeCreate");
  },
  created() {
      console.log("子组件Created");
  },
  beforeMount() {
      console.log("子组件beforeMount");
  },
  mounted() {
    const _this = this;
    window.WebViewJavascriptBridge.registerHandler("getImageStr", function(
      data,
      responseCallback
    ) {
      const dataJson = JSON.parse(data)
      alert("INDEX: data from Java: = " + dataJson+`${responseCallback}`);
      if(dataJson && dataJson.imageBase64Str){
        // _this.msg="send get dataJson from java, dataJson = " + dataJson.imageBase64Str;
        _this.imgSrc = "data:image/png;base64,"+dataJson.imageBase64Str;
      }
    });
  },
  methods: {
  formJava(){
    const _this = this;
      window.WebViewJavascriptBridge.callHandler(
        'updaloadImage', {'param': "str1"}
        , function(responseData) {
          alert(`${responseData}`);
           if(responseData && responseData.imageBase64Str){
            _this.msg="send get responseData from java, data = " + responseData.imageBase64Str;
            _this.imgSrc = responseData.imageBase64Str;
           }
        }
    );
  }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
