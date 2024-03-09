<template>
  <div>
    <h1>{{ title }}</h1>
    <div>
      <img :src="files[tmslider]" @mousemove="imgMouseMove" @mousedown="imgMouseDown" @mouseup="imgMouseUp" @contextmenu="(e)=>{ e.preventDefault() }">
    </div>
    <div>
      <div class="yoko">
        <p>Time</p>
        <progress id="prog" :max="maxtime" :value="times[tmslider]" class="slider"></progress>
        <p>{{ times[tmslider] }}</p>
      </div>
      <div class="yoko">
        <p>Frame</p>
        <input type="range" :max="files.length - 1" step="1" v-model="tmslider" class="slider">
        <p>{{ tmslider }}</p>
        <button @click="playbutton">{{ pbutton }}</button>
        <button @click="speedbutton">x{{ speed }}</button>
      </div>
    </div>
  </div>
</template>

<script>
//import VueTypes from 'vue-types'

export default {
  name: 'InsituViz',
  props: {
    title: String,
    src: String //VueTypes.string.isRequired.def('')
  },
  data() {
    return {
      "tmslider": 0,
      "files": [],
      "times": [],
      "maxtime": 0,
      "pbutton": "Play",
      "timeid": null,
      "speed": 1
    }
  },
  methods: {
    init()
    {
      this.times.length = 0
      this.files.length = 0
      this.maxtime = 0
    },
    loadData(srccsv) {
      var spl = srccsv.split("/")
      var srcpath = (spl.slice(0, spl.length - 1)).join("/")
      fetch(srccsv)
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
        //console.log(headers)
        const rowsRaw = CSVRaw.slice(endOfFirstLineIndex + 1);
        const rowsNoCells = rowsRaw.split("\n");
        const rows = rowsNoCells.map(row => row.split(","));
        var i
        if (headers.length == 3) { // single: Time,Frame,FILE
          for (i in rows){
            //console.log(rows[i][0])
            if (rows[i][0] && rows[i][2]) {
              this.times.push(parseInt(rows[i][0]) | 0)
              this.files.push(srcpath + '/' + rows[i][2])
            }
          }
        } else { // multi: Time,Frame,r,Phi,Theta,FILE
          for (i in rows){
              //console.log(rows[i][0])
              if (rows[i][0] && rows[i][5]) {
                this.times.push(parseInt(rows[i][0]) | 0)
                this.files.push(srcpath + '/' + rows[i][5])
              }
            }
        }

        const aryMax = function (a, b) { return Math.max(a, b) }
        this.maxtime = this.times.reduce(aryMax);
        console.log("max time:" + this.maxtime)
      })
      .catch(error => {
        // エラーが発生した場合の処理
        console.error('There was a problem with the fetch operation:', error);
      });
    },
    playbutton()
    {
      if (this.timeid == null) {
        this.pbutton = "Stop"
        this.play()
      } else {
        this.pause()
        this.pbutton = "Play"
      }
    },
    speedbutton()
    {
      if (this.speed == 1) {
        this.speed = 2
      } else if (this.speed == 2) {
        this.speed = 4
      } else if (this.speed == 4) {
        this.speed = 8
      } else if (this.speed == 8) {
        this.speed = 16
      } else if (this.speed == 16) {
        this.speed = 32
      } else if (this.speed == 32) {
        this.speed = 64
      } else {
        this.speed = 1
      }

      if (this.timeid != null) {
        this.play()
      }
    },
    play()
    {
      if (this.timeid == null) {
        this.timeid = setInterval(()=>{
          var x = parseInt(this.tmslider) + 1
          if (x > this.files.length - 1) {
            x = 0
          }
          this.tmslider = x
        }, 1000 / this.speed)
      } else {
        this.pause()
        this.play()
      }
    },
    pause()
    {
      if (this.timeid != null) {
        clearInterval(this.timeid)
        this.timeid = null
      }
    },
    imgMouseMove(event)
    {
      console.log(event.offsetX, event.offsetY)
    },
    imgMouseDown(event)
    {
      console.log(event.button)
      event.preventDefault()
    },
    imgMouseUp(event)
    {
      event.preventDefault()
    }

  },
  mounted() {
    this.init()
    console.log('load insitu csv:' + this.src)
    this.loadData(this.src)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.slider {
  width: 250px;
  margin-top: 20px;
  margin-bottom: 15px;
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
.yoko {
  flex-direction: row;
  display: flex;
}
</style>
