<template>
  <div id="app">
   <keep-alive> <router-view/></keep-alive>
    <main-tab-bar  v-if='$route.meta.showTab'/>
  </div>
</template>

<script>
 
import MainTabBar from './components/content/MainTabBar.vue'
export default{
  name:'app',
  components:{
    
    MainTabBar
  }
}
</script>

<style>
  @import "./assets/css/base.css";
</style>
