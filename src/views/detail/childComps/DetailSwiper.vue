<template>
        <swiper class="detail-swiper">
            <swiper-item 
                v-for="item in topImage" 
                :key="item"
                >
                <img :src="item">
            </swiper-item>
        </swiper>
</template>
<script>
import {Swiper, SwiperItem} from '../../../components/common/swiper/index'
export default {
   data() {
      return {
      }
   },
   components: {
       Swiper,
       SwiperItem
   },
   props: {
       topImage: {
           type: Array, 
           default() {
               return []
           }
       }
   }
}
</script>
<style>
    .detail-swiper {
        height: 300px;
        overflow: hidden;
    }
</style>
