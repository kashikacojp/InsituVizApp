<template>
  <div>
    <div class="wrap">
      <InsituVizApp title="My Data" src="data/data.csv"/>
      <!-- [github page sample]
      <InsituVizApp title="Single" src="data/single/data.csv"/>
      <InsituVizApp title="CameraPath" src="data/CameraPath/data.csv"/>
      <InsituVizApp title="CameraFocus" src="data/CameraFocus/data.csv"/>
      <InsituVizApp title="Multi" src="data/multi/data.csv"/>
      -->
    </div>
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
.wrap {
  display: flex;
  flex-wrap: wrap;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0px;
}
body {
  margin: 0px;
  padding: 0px;
}
</style>
