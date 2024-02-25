<template>
  <div>
    <h1>Insitu-vizualization</h1>
    <div>
      <img src="../assets/output_000000_000004.jpg">
    </div>
    <div>
      <p>Time</p>
      <input type="range" max="100" step="1" v-model="tmslider">
    </div>
  </div>
</template>

<script>
export default {
  name: 'InsituViz',
  props: {
  },
  data() {
    return {
      "tmslider": 10
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
