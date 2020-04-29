<template>
    <div id="detail" >
        <detail-nav-bar class="detail-nav" @isClick="titleClick"/>
        <scroll class="content" ref="scroll">
            <detail-swiper :top-image="topImage"/>
            <detail-base-info :goods="goods" />
            <detail-shop-info :shop="shop"/>
            <detail-goods-info :detail-info="detailInfo"/>
            <detail-param-info ref="params" :param-info="paramInfo" />
            <detail-comment-info ref="comment" :comment-info="commentInfo" />
            <goods-list ref="recommend" :goods="commends"/>
        </scroll>
    </div>
</template>
<script>
import DetailNavBar from './childComps/DetailNavBar'
import {getDetail, Goods, Shop, GoodsParpm, getCommend} from '../../network/detail'
import DetailSwiper from './childComps/DetailSwiper'
import DetailBaseInfo from './childComps/DetailBaseInfo'
import DetailShopInfo from './childComps/DetailShopInfo'
import Scroll from '../../components/common/scroll/Scroll'
import DetailGoodsInfo from './childComps/DetailGoodsInfo'
import DetailParamInfo from './childComps/DetailParamInfo'
import DetailCommentInfo from './childComps/DetailCommentInfo'
import GoodsList from '../../components/content/goods/GoodList'
export default {
   data() {
      return {
          iid: null,
          topImage: [],
          goods: {},
          shop: {},
         detailInfo: {},
         paramInfo: {},
         commentInfo: {},
         commends: [],
         themeTopYs: []
      }
   },
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
   created(){
       this.iid = this.$route.params.iid
        getDetail(this.iid).then(res => {
           this.topImage = res.result.itemInfo.topImages
           this.goods = new Goods(res.result.itemInfo, res.result.columns, res.result.shopInfo.services)
           this.shop = new Shop(res.result.shopInfo)
           this.detailInfo = res.result.detailInfo
           this.paramInfo = new GoodsParpm(res.result.itemParams.info, res.result.itemParams.rule)
           if(res.result.rate.cRate !== 0) {
               this.commentInfo = res.result.rate.list[0]
           }
        })   
        getCommend(this.iid).then(res => {
            this.commends = res.data.list
        })
   },
   updated(){
       this.themeTopYs=[]
       this.themeTopYs.push(0);
//        this.themeTopYs.push(this.$refs.params.$el.offsetTop)
//        this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
//        this.themeTopYs.push(this.$refs.recommend.$el.offsetTop)
//        console.log(this.themeTopYs)
   },
   methods: {
       imageLoad(){
           this.$refs.scroll.refresh()
       },
       titleClick(index){
        //    this.$refs.scroll.scrollTo(0, -this.themeTopYs(index), 200)
       }
   }
}
</script>
<style >
    #detail {
        position: relative;
        z-index: 9;
        background-color: #fff;
        height: 100vh;
    }
    .content {
        height: calc(100% - 44px);
    }
    .detail-nav {
        position: relative;
        z-index: 9;
        background-color: #fff;
    }
</style>
