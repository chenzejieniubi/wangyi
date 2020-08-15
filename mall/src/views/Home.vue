<template>
<!--  -->
  <div id="home" class="home">
    <van-search v-model="searchData" placeholder="商品搜索 共239款好物"  input-align="center"/>
    <van-swipe :autoplay="3000" :width="375" :height="200"> 
       <!-- 为什么要动态？？ -->
      <van-swipe-item v-for="(image, index) in images" :key="index">
        <img class="swiperimg" v-lazy="image.image_url" />
      </van-swipe-item>
    </van-swipe>

    <!-- 5个图标 -->
    <van-grid :column-num="5">
      <van-grid-item v-for="(item,index) in channel" :key="index" :icon="item.icon_url" :text="item.name" />
    </van-grid>


    <div class="cellspan">
    <!-- <van-cell center   value="品牌制造商直供"  size="large"  /> -->
      <div class="top">品牌制造商直供</div>
      <div class="brandlist">
      <van-grid :column-num="2">
        <van-grid-item v-for="(item1,index1) in brandList" :key="index1" icon="photo-o" text="文字">
          <van-image lazy-load :src="item1.new_pic_url" />
          <h4 class="title">{{item1.name}}</h4>
          <p class="price">{{item1.floor_price}}元起</p>
        </van-grid-item>
      </van-grid>
      </div>
    </div>





    <div class="cellspan">
    <!-- <van-cell center   value="新品首发"  size="large"  /> -->
      <div class="top">新品首发</div>
      <div class="newlist">
      <van-grid :column-num="2">
        <van-grid-item v-for="(item2,index2) in newGoodsList" :key="index2" icon="photo-o" text="文字">
          <van-image lazy-load :src="item2.list_pic_url" />
          <h4 class="title">{{item2.name}}</h4>
          <p class="price">{{item2.retail_price}}元起</p>
        </van-grid-item>
      </van-grid>
      </div>
    </div>



    

    <div class="cellspan">
    <!-- <van-cell center   value="人气面板"  size="large"  /> -->
      <div class="top">人气面板</div>
      <div class="hotlist">
      <van-card v-for="(item3,index3) in hotGoodsList" :key="index3"
        :price="item3.retail_price"
        :desc="item3.goods_brief"
        :title="item3.name"
        :thumb="item3.list_pic_url"
      />
      </div>
    </div>
    <div style="height:50px;"></div>
    
    <tab-btn> </tab-btn>



  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'
import api from '../assets/config/api'
import Vue from 'vue';
import { Lazyload } from 'vant';
import tabBtn from '@/components/tabBtn.vue';

Vue.use(Lazyload);
export default {
  name: 'home',
  data:function(){
    return {
      searchData:'',
      data:{},
      tabActive:0
    }
  },
  computed: {
    images:function(){
      if(typeof this.data.banner == 'object'){
        return this.data.banner
      }else{
        return []
      }
    },
    channel:function(){
      if(typeof this.data.channel == 'object'){
        return this.data.channel
      }else{
        return []
      }
    },
    brandList:function(){
      if(typeof this.data.brandList == 'object'){
        return this.data.brandList
      }else{
        return []
      }
    },
    newGoodsList:function(){
      if(typeof this.data.newGoodsList == 'object'){
        return this.data.newGoodsList
      }else{
        return []
      }
    },
    hotGoodsList:function(){
      if(typeof this.data.hotGoodsList == 'object'){
        return this.data.hotGoodsList
      }else{
        return []
      }
    },
  },
  components: {
    tabBtn
  },
 async mounted() {
    //console.log(api)
    let res = await axios.get(api.IndexUrl)
    //console.log(res.data)
    this.data = res.data.data;
  },
}
</script>

<style lang="less">
  #home{
    .van-grid-item{
      overflow: hidden;
    }
    .swiperimg{
      width: 375px;
      height: 200px;
    }
    .cellspan .top{
      margin: 0 auto;
      padding: 20px;
    }
    .brandlist{
      .van-grid-item__content{
        padding: 0;
      }
      .van-image{
        border: 1px solid #ffffff;
      }
      .title{
        position: absolute;
        top: 20px;
        left: 10px;
      }
      .price{
        position: absolute;
        top: 40px;
        left: 10px;
        font-size: 14px;
        color: #999;
      }
    }
    .newlist{
      .title{
        width: 90%;
        overflow: hidden;
        white-space: nowrap;
      }
      .price{
        font-size: 14px;
        color: #999;
      }
    }
  }
</style>
