<template>
    <div>
        <nav-bar>
            <div slot="left" class="back" @click="backClick">
                <img src="../../../assets/img/common/back.svg">
            </div>
            <div slot="center" class="title">
                <div 
                    v-for="(item, index) in titles" 
                    :key="index" 
                    class="titleItem"
                    :class="{active:index===currentIndex}"
                    @click="isClick(index)"
                >
                    {{ item }}
                </div>
            </div>
        </nav-bar>
    </div>
</template>
<script>
import NavBar from '../../../components/common/navbar/NavBar'

export default {
   data() {
      return {
          titles: ['商品', '参数', '评论', '推荐'],
          currentIndex: 0
      }
   },
   components: {
       NavBar
   },
   methods: {
       isClick(index){
           this.currentIndex = index
           this.$emit('isClick', index)
       },
       backClick(){
           this.$router.back()
       }
   }
}
</script>
<style>
    .title {
        display: flex;
        font-size: 13px;
    }
    .titleItem {
        flex: 1;
    }
    .active {
        color: var(--color-high-text);
        /* color: red */
    }
    .back img {
        margin-top: 10px;
    }
</style>
