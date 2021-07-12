<template>
  <div id="home">
    
   <nav-bar  class="home-nav">
     <template v-slot:center>
       <div>购物街</div>
     </template>
   </nav-bar>
   <tab-control class="tab-control" 
  @tabclick="tabclick"
  :titles="['流行','新款','精选']"
  ref="tabControl1"
  v-show="isTabFixed"
  />
  <scroll class="content"
          ref="scroll" 
          :probe-type='3'
          :pullUpLoad ='true'
           @scroll="contentScroll"
           @pullingUp="loadMore"
           >
    <home-swiper :banners="banners"/>
  <recommend-view :recommends="recommends"/>
  <feature-view />
  <tab-control class="tab-control" 
  @tabclick="tabclick"
  :titles="['流行','新款','精选']"
  ref="tabControl2"
  />
    <goods-list :goods="showGoods"/>
  </scroll>

  <back-top @click.native="backClick" v-show="isShowBackTop"/>
  </div>
</template>

<script>



import HomeSwiper from './childComponents/HomeSwiper.vue'
import RecommendView from './childComponents/RecommendView.vue'
import FeatureView from './childComponents/FeatureView.vue'

import NavBar from '@/components/common/navbar/NavBar.vue';
import TabControl from '@/components/content/tabControl/TabControl.vue'
import GoodsList from '@/components/content/goods/GoodsList.vue'
import Scroll from '@/components/common/scroll/Scroll.vue'
import BackTop from '../../components/common/backTop/BackTop.vue';

import {getHomeMultidata,getHomeGoods} from "@/network/home"




  export default {
    name: "Home",
    components:{
      NavBar,
      HomeSwiper,
      RecommendView,
      FeatureView,
       TabControl,
       GoodsList,
       Scroll,
       BackTop
      
        
        
       
    },
    data(){
      return{
        banners:[],
        recommends:[],
        goods:{
          'pop':{page:0,list:[]},
          'new':{page:0,list:[]},
          'sell':{page:0,list:[]},
        },

        currentType :'pop',
        isShowBackTop:false,
        tabOffsetTop:0,
        isTabFixed:false,
        saveY:0,
      }
    },
    computed:{
        showGoods(){
          return this.goods[this.currentType].list
        }
    },
    created(){
      //1.请求多个数据
      this.getHomeMultidata()
      //2.请求商品数据
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')
    },
    updated(){
      //获取tabControl的offsetTop
     this.tabOffsetTop=this.$refs.tabControl2.$el.offsetTop
    },
    destroyed(){
      console.log('des');
    },
    activated(){
      this.$refs.scroll.refresh()
      this.$refs.scroll.scrollTo(0,this.saveY,0)
      
    },
    deactivated(){
      this.saveY =this.$refs.scroll.getScrollY()
    },
    methods:{
      /*事件监听相关方法*/ 
      tabclick(index){
        switch(index){
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
            break

        }
        this.$refs.tabControl1.currentIndex = index;
        this.$refs.tabControl2.currentIndex = index;
      },
      backClick(){
        this.$refs.scroll.scrollTo(0,0)
      },
      contentScroll(position){
        //1.判断BackTop是否显示
        this.isShowBackTop = position.y < -1000

        //2.决定tabControl是否吸顶（position:fixed)
        this.isTabFixed = (-position.y) > this.tabOffsetTop
      },
      //下拉加载更多
      loadMore(){
       this.getHomeGoods(this.currentType)

       
      },
      /**网络请求相关方法 */
      getHomeMultidata(){
        getHomeMultidata().then(res =>{
        this.banners = res.data.banner.list
         this.recommends = res.data.recommend.list
      })
      },
      getHomeGoods(type){
        const page = this.goods[type].page + 1
        getHomeGoods(type,page).then(res=>{
         
        this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1
        this.$refs.scroll.finishPullUp()
        this.$refs.scroll.scroll.refresh()
      })
      }
    }
  }
</script>

<style scoped>
#home{

  height: 100vh;
}
.home-nav{
 background-color: var(--color-tint);
 color: white;

 /* position:sticky;
 top:0;
 z-index: 9;*/
} 
.tab-control{
  position: relative;
  z-index: 9;
}
/* .fixed{
  position: fixed;
  left: 0;
  top:44px;
  right:0;
} */
.content{
 height: calc(100% - 93px);
 overflow: hidden;
 
}
</style>
