<template>
    <div class="tab-control">
        <div v-for="(item, index) in title" :key="item.length" class="tab-control-item"
        :class="{active:index===currentindex}" @click="isClick(index)">
            <span>{{  item  }}</span>
        </div>
    </div>
</template>
<script>
export default {
   data() {
      return {
          currentindex: 0
      }
   },
   props: {
       title: {
           type: Array, 
           default(){
               return []
           }
       }
   },
   methods: {
       isClick(index){
           this.currentindex = index,
           this.$emit('isClick', index)
       }
   }
}
</script>
<style>
    .tab-control {
        display: flex;
        /* text-align: center; */
        background-color: #fff;
        z-index: 999;
    }
    .tab-control-item {
        flex: 1;
        height: 40px;
        text-align: center;
        line-height: 40px;
    }
    .tab-control-item span{
        padding-bottom: 5px;
    }
    .active {
        color: var(--color-high-text)
    }
    .active span {
        border-bottom: 3px solid var(--color-high-text);
    }
</style>
