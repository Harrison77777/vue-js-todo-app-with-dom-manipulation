<template>
  <div id="app">
    
    <router-view/>
  </div>
</template>

<script>
import router from './router/index'
export default {
  name: 'App',
  comments:{
    router
  }
}
</script>

<style>

</style>
