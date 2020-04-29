<template>
  <div id="app">
    <keep-alive exclude="Detail">
    <router-view></router-view>
    </keep-alive>
    <main-tab-bar />
  </div>
</template>
<script>
  import MainTabBar from './components/content/maintarbar/MainTarBar'
  export default {
    name: 'app',
    components: {
      MainTabBar
    }
  }
</script>

<style lang="stylus">
  @import url(./assets/css/base.css);
</style>
