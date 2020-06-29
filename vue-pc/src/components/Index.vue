<template>
  <div id="applicationsIndex" class="comContainer" style="padding-top: 0px;">
    <!-- <indexhd :active="'0'" @sendUserInfo="handleGetUser"></indexhd> -->
    <div class="comCenter" style="margin-top: 0px;margin-bottom: 0px;">
      <div class="bannerOnelevel">
        <div class="banner">
          <div class="bannerCon" :class="{'bannerBg':indexBanners.length<1,}">
            <!-- <el-carousel :interval="5000" arrow="always" v-if="indexBanners.length" trigger="click">
              <el-carousel-item
                v-for="(indexBanner,index) in indexBanners"
                :style="{'background':'url('+indexBanner.bannerImageUrl+')no-repeat center center scroll'}"
                :key="index"
              >
                <a v-if="indexBanner.bannerTargetUrl" :href="indexBanner.bannerTargetUrl"></a>
              </el-carousel-item>
            </el-carousel> -->
            <swiper  ref="mySwiper" class="swiper-no-swiping">
                <swiper-slide  
                    v-for="(indexBanner,index) in indexBanners"
                    :style="{'height':'465px','background':'url('+indexBanner.bannerImageUrl+')no-repeat center center scroll'}"
                    :key="index"
                >
                   <a 
                      :style="{'height':'465px','display':'block'}"
                      target='_blank'
                      v-if="indexBanner.bannerTargetUrl" 
                      :href="indexBanner.bannerTargetUrl">
                   </a> 
                </swiper-slide>
                <div class="swiper-pagination" v-show='indexBanners.length > 1'  slot="pagination"></div>
            </swiper>
            <!-- <swiper :options="swiperOption" class="swiper-container" >
       
        <swiper-slide class="swiper-item" v-for='item of swiperList' :key='item.id'>
            <img class='swiper-img' :src='item.imgUrl' alt="去哪儿门票" />
        </swiper-slide>
       
        <div class="swiper-pagination"  slot="pagination"></div>
    </swiper> -->
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import 'swiper/css/swiper.css'    //在全局没引入，这里记得要！
import { swiper, swiperSlide } from 'vue-awesome-swiper'
import data from '../jsonData.json'
export default {
  name: 'Index',
  data () {
    return {
      msg: '首页',
      indexBanners: [],
       swiperOption:{
        observer:true,
        // loop:true,
        observeParents:true,
        autoplay: {
            delay: 5000,
            disableOnInteraction: false,
        },
        pagination: {
            el: '.swiper-pagination',
            clickable :true
        },
        on:{
            paginationRender:function(){
                if(this.pagination.bullets){
                    for(var i=0;i<this.pagination.bullets.length;i++){
                        this.pagination.bullets[i].onmouseover=function(){
                            this.click()
                        }
                    }
                }
            }
        }
    },
    }
  },
  // 组件注册方式
  components: {
    swiper, swiperSlide
  
  },
  computed:{
    swiper(){
          return this.$refs.mySwiper.swiper
    },
  },
  created(){
    this.indexBanners = data.responseData.sort(
            this.compare("bannerOrder")
          );
    console.log(data,'data--')
  },
   mounted() {
    
    this.getBannerLists();
  },
  methods:{
     // banner排序
    compare(property) {
      return function(a, b) {
        var value1 = a[property];
        var value2 = b[property];
        return value2 - value1;
      };
    },
    // 动态获取banner
    getBannerLists() {
      this.indexBanners = data.responseData.sort(
            this.compare("bannerOrder")
          );
     
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
  li {
  display: inline-block;
  margin: 0 10px;
  }
}

a {
  color: #42b983;
}
</style>
