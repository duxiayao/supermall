<template>
    <div class="recommend">
        <div v-for="item in recommends" :key="item" class="recommend-item">
            <a :href="item.link">
                <img :src="item.image">
                <h3>{{  item.title }}</h3>
            </a>
        </div>
    </div>
</template>
<script>
export default {
   data() {
      return {
      }
   },
   props: {
       recommends: {
           type: Array,
           default(){
               return []
           }
       }
   }
}
</script>
<style>
    .recommend {
        display: flex;
        text-align: center;
        border-bottom: 8px solid #eee;
    }
    .recommend-item {
        flex: 1;
    }
    .recommend-item img {
        width: 65px;
        height: 65px;
    }
</style>
