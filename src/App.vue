<template>
  <div id="app">
    <weather/>
  </div>
</template>

<script>
import Weather from './components/Weather.vue'

export default {
  name: 'App',
  components: {
    'weather': Weather
  }
}
</script>

<style>

</style>
