<template>
  <div class="goods-item" @click = 'itemClick'>
      <img :src="showImage"/>
      <div class="goods-info">
          <p>{{ goodsItem.title}}</p>
          <span class="price">{{ goodsItem.price}} </span>
          <span class="collect">{{ goodsItem.cfav}}</span>
      </div>
  </div>
</template>

<script>
export default {
  name: 'GoodsListItem',
  props:{
      goodsItem:{
          type:Object,
          default(){
              return {}
          }
      }
  },
  computed:{
      showImage(){
          return this.goodsItem.image || this.goodsItem.show.img
      }
  },
  methods:{
      itemClick(){
          this.$router.push({
              path:'./detail',
              query:{
                  iid:this.goodsItem.iid
              }
          })
      }
  }

}

</script>

<style scoped>
.goods-item{
    width: 48%;
    padding-bottom: 9px;
}
.goods-item img{
    width: 100%;
    border-radius: 5px;
}
.goods-info{
     display: flex;
    align-items: center; /*垂直方向居中*/
    justify-content: center;  /*水平方向居中*/
    flex-wrap: wrap;/*换行显示*/ 
    font-size: 12px;
}
.goods-info p{
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
}
.goods-info .price{
   margin-right: 2px;
    color: var(--color-high-text);
}

.goods-info .collect::before {
    display: inline-block;
    content: '';
    width: 14px;
    height: 14px;
    
     background-size: contain;
    background: url(~@/assets/img/common/collect.svg) 0 0/14px 14px;
}
</style>