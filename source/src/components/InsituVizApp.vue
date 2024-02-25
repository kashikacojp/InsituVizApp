<template>
  <div>
    <h1>Insitu-vizualization</h1>
    <div>
      <img :src="files[parseInt(tmslider * (files.length-1) / 100)]">
    </div>
    <div>
      <p>Time</p>
      <input type="range" max="100" step="1" v-model="tmslider" class="slider">
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
      "tmslider": 10,
      "files": []
    }
  },
  mounted() {
    console.log('A')
    fetch('/data/data.csv')
    .then(response => {
      // レスポンスが成功したかどうかを確認
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      // レスポンスからテキストデータを取得して返す
      return response.text();
    })
    .then(CSVRaw => {
      const endOfFirstLineIndex = CSVRaw.indexOf("\n");
      const headerRawLine = CSVRaw.slice(0, endOfFirstLineIndex);
      const headers = headerRawLine.split(",");
      console.log(headers)

      const rowsRaw = CSVRaw.slice(endOfFirstLineIndex + 1);
      const rowsNoCells = rowsRaw.split("\n");
      const rows = rowsNoCells.map(row => row.split(","));
      console.log(rows)

      for (var i in rows){
        console.log(rows[i][2])
        this.files.push('../../data/' + rows[i][2])
      }
    })
    .catch(error => {
      // エラーが発生した場合の処理
      console.error('There was a problem with the fetch operation:', error);
    });

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.slider {
  width: 250px;
}
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
