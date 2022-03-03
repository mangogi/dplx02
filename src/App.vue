<template>
  <div id="app">
    <MainPage/>
  </div>
</template>

<script>
import MainPage from './views/ flowTalents/MainPage.vue'

export default {
  name: 'app',
  components: {
    MainPage
  }
}
</script>

<style>
/* 高100% */
#app{
  height: 100%;
}
</style>
