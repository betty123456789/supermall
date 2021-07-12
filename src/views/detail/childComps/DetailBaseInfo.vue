<template>
  <div v-if="Object.keys(goods).length !==0" class="base-info">
      <div class="info-title">{{goods.title}}</div>
      <div class="info-price">
          <span class="n-price">{{goods.newPrice}} </span>
          <span class="o-price">{{goods.oldPrice}} </span>
          <span v-if="goods.discount" class="discount">{{goods.discount}}</span>
      </div>
      <div class="info-other">
          <span>{{goods.columns[0]}}</span>
          <span>{{goods.columns[1]}}</span>
          <span>{{goods.services[0].name}}</span>
      </div>
      <div class="info-service">
          <span class="info-service-item" v-for="index in goods.services.length-1" :key="index">
              <img :src="goods.services[index].icon"/>
              <span>{{goods.services[index].name}}</span>
          </span>
      </div>
  </div>
</template>

<script>
export default {
  name: 'DetailBaseInfo',
    props:{
        goods:{
            type:Object,
            default(){
                return {}
            }
        }
    }
}

</script>

<style scoped>
.base-info{
    margin:15px 0px 0 8px;
    padding-bottom: 20px;
    border-bottom:  5px solid #f2f5f8;
}
.info-title{
    color: #222;
}
.info-price{
    margin: 12px 0 ;
}
.n-price{
    font-size: 24px;
    color:var(--color-high-text);
   
}
.o-price{
    font-size: 13px;
    text-decoration:line-through
}
.discount{
    font-size: 12px;
    padding: 2px 5px;
    color: #fff;
    background-color: var(--color-high-text);
    border-radius: 8px;
    margin-left: 5px;

    /*使元素上浮*/
    position: relative;
    top:-7px
}
.info-other{
    display: flex;
    justify-content: space-between;
    margin-top: 25px;
    margin-bottom: 25px;
    margin-right: 5px;
    padding-bottom: 5px;
     border-bottom:  1px solid #f2f5f8;
    font-size: 12px;
}
.info-service{
    display: flex;
    justify-content: space-between;
    font-size: 12px;
}
.info-service img{
    height: 14px;
    width: 14px;
}
</style>