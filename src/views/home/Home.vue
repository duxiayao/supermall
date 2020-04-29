<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <tab-control 
          :title="['流行', '新款', '精选']" 
          @isClick="isClick"  
          ref="tabControl1" class="tabcontrol" v-show="isTabFixed"/>
    <scroll 
        class="content1" 
        ref="scroll" 
        :probe-type="3" 
        @scroll="contentScroll" 
        :pull-up-load="true"
        @pullingUp="isPullUp">
      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature class="feature"></feature>
      <tab-control 
          :title="['流行', '新款', '精选']" 
          @isClick="isClick"  
          ref="tabControl" />
      <good-list :goods="goods[currentType].list" />
    </scroll>
    <back-top @click.native="backClick" v-show="isShowBackTop"/>
  </div>
</template>
<script>
import NavBar from "../../components/common/navbar/NavBar";
import HomeSwiper from "./childComps/HomeSwiper";
import RecommendView from "./childComps/RecommentView";
import Feature from "./childComps/Feature";
import TabControl from "../../components/content/tabcontrol/TabControl";
import GoodList from "../../components/content/goods/GoodList";
import BackTop from "../../components/content/backtop/BackTop"

import { getHomeMultidata, getHomeGoods } from "../../network/home";

import Scroll from "../../components/common/scroll/Scroll";
export default {
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] }
      },
      currentType: "pop",
      isShowBackTop: false,
      tabOffsetTop: 0,
      isTabFixed: false,
      saveY: 0
    };
  },
  components: {
    NavBar,
    HomeSwiper,
    RecommendView,
    Feature,
    TabControl,
    GoodList,
    Scroll,
    BackTop
  },
  created() {
    this.getHomeMultidata(), this.getHomeGoods("pop");
    this.getHomeGoods("new");
    this.getHomeGoods("sell");
      
  },
  activated(){
    this.$refs.scroll.scrollTo(0, this.saveY, 0)
    this.$refs.scroll.refresh()
  },
  deactivated(){
    this.saveY = this.$refs.scroll.getScrollY()
  },
  mounted(){
    const refresh = this.debounce(this.$refs.scroll.refresh, 300)
    this.$bus.$on('ItemImageLoad', ()=>{
      refresh()
    })
    
  },
  methods: {
    getHomeMultidata() {
      getHomeMultidata().then(res => {
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1;
      getHomeGoods(type, page).then(res => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;
        this.$refs.scroll.finishPullUp()
      });
    },
    isClick(index) {
      switch (index) {
        case 0:
          this.currentType = "pop";
          break;
        case 1:
          this.currentType = "new";
          break;
        case 2:
          this.currentType = "sell";
          break;
      }
      this.$refs.tabControl1.currentindex = index;
      this.$refs.tabControl.currentindex = index;
    },
    backClick(){
      this.$refs.scroll.scroll.scrollTo(0, 0, 500)
    },
    contentScroll(position){
      this.isShowBackTop = (-position.y) > 700

      this.isTabFixed = (-position.y) > this.tabOffsetTop;
    },
    isPullUp(){
      this.getHomeGoods(this.currentType)
    },
    debounce(func, delay){
      let timer = null
      return function(...args) {
        if(timer) clearTimeout(timer)
        timer = setTimeout(()=>{
          func.apply(this, args)
        }, delay)
      }
    },
    swiperImageLoad(){
      this.tabOffsetTop = this.$refs.tabControl.$el.offsetTop
    }
  }
};
</script>
<style >
#home {
  /* padding-top: 44px; */
  height: 100vh;
  position: relative;
  /* z-index: 999; */

}
.home-nav {
  background-color: var(--color-tint);
  color: #fff;
  /* position: relative; */
  /* left: 0;
  right: 0;
  top: 0;  */
  /* z-index: 999; */
}
.feature img {
  width: 375px;
  height: auto;
}

.content1 {
  overflow: hidden;
   position: absolute;
   top: 44px;
   bottom: 49px;
   right: 0;
   left: 0;
}
  .tabcontrol {
  position: relative;
  z-index: 999;
}
</style>
