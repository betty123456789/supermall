<template>
  <!-- 所有的item都展示同一个图片，同一个文字 -->
   <div class="tab-bar-item" @click="itemClick">
    <slot v-if="!isActive" name="item-icon"></slot>
    <slot v-else name="item-icon-active"></slot>
    <div :style="activeStyle"><slot name="item-text"></slot></div>
      <!-- <img src = '../../assets/img/tabbar/home.svg'>
      <div>首页</div> -->
      </div>
    
     
</template>

<script>
export default {
  name:"TabBarItem",
  props:{
    path:String,
    activeColor: {
      type:String,
      default:'red'
    }
  },
  data() {
    return {
    }
    },
  computed:{
    isActive() {
       return this.$route.path == this.path
  },
  activeStyle() {
    return this.isActive ? {color: this.activeColor} : {}
  }
  },
  methods:{
    itemClick(){
     this.$router.replace(this.path).catch((error) => error);
    }
  }
}
</script>

<style>
.tab-bar-item{
  flex: 1;
  text-align: center;
  height: 49px;
  font-size:14px ;
}

.tab-bar-item img{
  width: 24px;
  height: 24px;
  margin-top: 3px;
}
</style>