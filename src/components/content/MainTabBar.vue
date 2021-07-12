<template>
  <tabbar>
      <tabbar-item path="/home" activeColor="pink">
        <template v-slot:item-icon>
          <img src="~@/assets/img/tabbar/home.svg" alt="">
        </template>
         <template v-slot:item-icon-active>
          <img src="~@/assets/img/tabbar/home_active.svg" alt="">
        </template>
        <template v-slot:item-text>
          <div >首页</div>
        </template>
      </tabbar-item>
      <tabbar-item path="/category" activeColor="pink">
         <template v-slot:item-icon>
          <img src="~@/assets/img/tabbar/category.svg" alt="">
        </template>
        <template v-slot:item-icon-active>
          <img src="~@/assets/img/tabbar/category_active.svg" alt="">
        </template>
        <template v-slot:item-text>
          <div >分类</div>
        </template>
      </tabbar-item>
      <tabbar-item path="/cart" activeColor="pink">
         <template v-slot:item-icon>
          <img src="~@/assets/img/tabbar/shopcart.svg" alt="">
        </template>
         <template v-slot:item-icon-active>
          <img src="~@/assets/img/tabbar/shopcart_active.svg" alt="">
        </template>
        <template v-slot:item-text>
          <div >购物车</div>
        </template>
      </tabbar-item>
      <tabbar-item path="/profile" activeColor="pink">
         <template v-slot:item-icon>
          <img src="~@/assets/img/tabbar/profile.svg" alt="">
        </template>
         <template v-slot:item-icon-active>
          <img src="~@/assets/img/tabbar/profile_active.svg" alt="">
        </template>
        <template v-slot:item-text>
          <div >我的</div>
        </template>
      </tabbar-item>
    </tabbar>
</template>

<script>
import tabbar from '../../components/common/tabbar/tabbar.vue'
import tabbarItem from '../../components/common/tabbar/tabbarItem.vue'

export default {
    name:'MainTabBar',
     components:{
    tabbar,
    tabbarItem
  }
}
</script>

<style>

</style>