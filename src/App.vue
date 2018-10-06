<template>
  <div id="app">
    <leftNavbar></leftNavbar>
    <div class="main">
      <router-view/>
    </div>
    
  </div>
</template>

<script>
import leftNavbar from '@/components/commom/leftNavbar.vue'

export default {
  name: 'App',
  components: {
    leftNavbar
  }
}
</script>

<style>
html,body{
  margin: 0;
  padding: 0;
}

#app {
  font-family: 'Microsoft YaHei','Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0px;
  height: 100%;
}

.main{
  height: 100%;
  background-color: #eff2f7;
  float: left;
  width: 95%;
  overflow: hidden;
}
</style>
