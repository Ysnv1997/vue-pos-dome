<template>
  <div id="app">
    <left-nav></left-nav>
    <router-view/>
  </div>
</template>
<script>
  import LeftNav from './components/commped/LeftNav.vue'
  export default{
    components:{
      LeftNav
    }
  }
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
</style>
