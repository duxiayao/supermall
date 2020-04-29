<template>
    <div class="GoodList">
        <!-- {{ goods }} -->
        <good-list-item v-for="item in goods" :key="item" :goods-item="item" />
    </div>
</template>
<script>
import GoodListItem from './GoodListItem'
export default {
   data() {
      return {
      }
   },
   props: {
       goods: {
           type: Array,
           default(){
               return []
           }
       }
   },
   components: {
       GoodListItem
   }
}
</script>
<style>
 .GoodList {
     display: flex;
     flex-wrap: wrap;
     justify-content: space-around;
 }
</style>
