<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1>{{msg}}</h1>
    <button @click="formJava">从java来</button>
  </div>
</template>

<script>
import  './config/bridge'

export default {
  name: 'App',
  data () {
    return {
      msg: 'safsd'
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
   window.WebViewJavascriptBridge.registerHandler("getImageStr", function(
      data,
      responseCallback
    ) {
      alert("APPTEXT: data from Java: = " + data);
    });
  },
  methods: {
  formJava(){
      window.WebViewJavascriptBridge.callHandler(
        'updaloadImage'
        , {'param': "str1"}
        , function(responseData) {
            this.msg="send get responseData from java, data = " + responseData;
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
