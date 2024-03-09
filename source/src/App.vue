<template>
  <div>
    <InsituVizApp title="Single" src="/data/single/data.csv"/>
    <InsituVizApp title="CameraPath" src="/data/CameraPath/data.csv"/>
    <InsituVizApp title="CameraFocus" src="/data/CameraFocus/data.csv"/>
    <InsituVizApp title="Multi" src="/data/multi2/data.csv"/>
  </div>
</template>

<script>
import InsituVizApp from './components/InsituVizApp.vue'

export default {
  name: 'App',
  components: {
    InsituVizApp
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
