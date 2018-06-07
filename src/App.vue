<template>
  <div id="app">
    <left></left>
    <div class="main">
      <router-view/>
    </div>   
  </div>
</template>

<script>
import left from '@/components/left/left'
export default {
  name: 'App',
  components:{
    left
  }
}
</script>

<style>
*{
    margin:0px;
    padding:0px;
}
html,body{
  height:100%;
}
ul li{
    list-style:none;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height:100%;
}
.main{
  float:left;
  width:92%;
  background:#f2f2f2;
  height:100%;
}
</style>
