<template>
 <div id="detail">
     <detail-nav-bar class="detail-nav" @titleClick="titleClick"></detail-nav-bar>
     <scroll class="content" ref="scroll" :pull-up-load="true">
     <detail-swiper :topImages="topImages"></detail-swiper>
     <detail-base-info :goods="goods"></detail-base-info>
     <detail-shop-info :shop="shop"></detail-shop-info>
     <detail-goods-info :detail-info="detailInfo" @detailImageLoad='imageLoad'></detail-goods-info>
     <detail-param-info :param-info="paramInfo" ref="params"/>
     <detail-comment-info  ref="comment" :comment-info = "commentInfo"/>
     <goods-list  ref="recommend" :goods="recommend"/>
     </scroll>
    
 </div>
</template>

<script>
import DetailNavBar from './childComps/DetailNavBar.vue'
import DetailSwiper from './childComps/DetailSwiper.vue'
import DetailBaseInfo from './childComps/DetailBaseInfo.vue'
import DetailShopInfo from './childComps/DetailShopInfo.vue'
import DetailGoodsInfo from './childComps/DetailGoodsInfo.vue'
import DetailParamInfo from './childComps/DetailParamInfo.vue'
import DetailCommentInfo from './childComps/DetailCommentInfo.vue'
import GoodsList from '../../components/content/goods/GoodsList.vue'
import Scroll from '../../components/common/scroll/Scroll.vue'

import {getDetail,Goods,shop, GoodsParam,getRecommend} from "@/network/detail"
import { debounce } from "@/common/utils";
// import {imgListenerMixin} from "@/common/mixin"



export default {
  
  name: 'Detail',
  components: {
       DetailNavBar,
       DetailSwiper,
       DetailBaseInfo,
    DetailShopInfo,
    Scroll,
     DetailGoodsInfo,
     DetailParamInfo,
     DetailCommentInfo,
    GoodsList
     },
    //  mixins:[imgListenerMixin],
    data(){
        return{
            iid:null,
            topImages:[],
            goods:{},
            shop:{},
            detailInfo:{},
            paramInfo:{},
            commentInfo:{},
            recommend:[],
            themeTopYs:[],
            getThemeTopY:null
            // refresh:null
        }
    },
    updated(){
         this.$refs.scroll.refresh();

         
    },
    activated(){
        //1.保存传入的iid
        this.iid = this.$route.query.iid
        // 2.根据iid请求详情数据
        getDetail(this.iid).then(res=>{
            // console.log(res);
            //1.获取顶部的图片轮播数据
            const data = res.result;
            this.topImages = data.itemInfo.topImages

            //2.获取商品信息
            this.goods = new Goods(data.itemInfo,data.columns,data.shopInfo.services)
            //3.创建店铺信息
            this.shop = new shop(data.shopInfo)

            //4.获取商品详情信息
            this.detailInfo = data.detailInfo
            
            //5.获取参数信息
            this.paramInfo = new GoodsParam(data.itemParams.info,data.itemParams.rule)

            //6.取出评论的信息
            if(data.rate.cRate !==0){
                this.commentInfo = data.rate.list[0]
            }

            this.$nextTick(()=>{
                this.themeTopYs = []

                this.themeTopYs.push(0)
         this.themeTopYs.push(this.$refs.params.$el.offsetTop - 44)
         this.themeTopYs.push(this.$refs.comment.$el.offsetTop - 44)
         this.themeTopYs.push(this.$refs.recommend.$el.offsetTop - 44)
            })
        })

        //3.请求推荐数据
        getRecommend().then(res=>{
            this.recommend = res.data.list
        })

        //4.给getThemeTopY赋值
        this.getThemeTopY = debounce(()=>{
         this.themeTopYs = []

        this.themeTopYs.push(0)
         this.themeTopYs.push(this.$refs.params.$el.offsetTop )
         this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
         this.themeTopYs.push(this.$refs.recommend.$el.offsetTop )
         
        },100)
    },
    methods:{
        imageLoad(){
            this.$refs.scroll.refresh();
            this.getThemeTopY();
           
        },
        titleClick(index){
           this.$refs.scroll.scrollTo(0,-this.themeTopYs[index],200)
        }
    }
}

</script>

<style scoped>
#detail{
    position: relative;
    z-index: 9;
    background-color: #fff;
    height: 100vh;
}
.detail-nav{
    position: relative;
    z-index: 9;
}
.content{
    height: calc(100% - 44px);
    overflow: hidden;
}
</style>