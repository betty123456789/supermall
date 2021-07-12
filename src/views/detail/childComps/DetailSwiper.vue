<template>
  <div class="detail-swiper">
      <swiper class="aaa">
          <swiper-item v-for="(item,index) in topImages" :key="index">
              <img :src="item"/>
          </swiper-item>
      </swiper>
  </div>
</template>

<script>
import {Swiper,SwiperItem} from '@/components/common/swiper/index'


export default {
  name: 'DetailSwiper',
  components:{
      Swiper,
      SwiperItem
    
  },
    props:{
        topImages:{
            type:Array,
            default(){
                return []
            }
        }
    }
}

</script>

<style scoped>
.aaa {
    height: 300px;
    overflow: hidden;
}
</style>