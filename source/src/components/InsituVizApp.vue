<template>
  <div class="component">
    <h1>{{ title }}</h1>
    <div>
      <img :src="files(tmslider)"
       @mousemove="imgMouseMove"
       @mousedown="imgMouseDown"
       @mouseup="imgMouseUp"
       @contextmenu="(e)=>{ e.preventDefault() }"
       class="imagecomp">
    </div>
    <div>
      <div class="yoko">
        <p class="txt">Time</p>
        <progress id="prog" :max="maxtime" :value="times(tmslider)" class="slider"></progress>
        <p class="txt">{{ times(tmslider) }}</p>
      </div>
      <div class="yoko">
        <p class="txt">Frame</p>
        <input type="range" :max="datum.length - 1" step="1" v-model="tmslider" class="slider">
        <p class="txt">{{ tmslider }}</p>
        <button class="btn" @click="playbutton">{{ pbutton }}</button>
        <button class="btn" @click="speedbutton">x{{ speed }}</button>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'InsituViz',
  props: {
    title: String,
    src: String
  },
  data() {
    return {
      "tmslider": 0,
      "singlefiles": [],
      "multifiles": [],
      "maxtime": 0,
      "pbutton": "Play",
      "timeid": null,
      "speed": 1,
      "mousedrag": false
    }
  },
  computed: {
    datum() {
      if (this.multifiles.length > 0) {
        return this.multifiles
      } else if (this.singlefiles.length > 0) {
        return this.singlefiles
      } else {
        return []
      }
    }
  },
  methods: {
    init()
    {
      this.singlefiles.length = 0
      this.multifiles.length = 0
      this.maxtime = 0
      this.mousedrag = false
    },
    files(i) {
      if (this.multifiles.length > 0) {
        return this.multifiles[i].file
      } else if (this.singlefiles.length > 0) {
        return this.singlefiles[i].file
      } else {
        return ""
      }
    },
    times(i) {
      if (this.multifiles.length > 0) {
        return this.multifiles[i].time
      } else if (this.singlefiles.length > 0) {
        return this.singlefiles[i].time
      } else {
        return 0
      }
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
        var i, tm
        if (headers.length == 3) { // single: Time,Frame,FILE
          for (i in rows){
            //console.log(rows[i][0])
            if (rows[i][0] && rows[i][2]) {
              tm = parseInt(rows[i][0]) | 0
              this.singlefiles.push({
                "time": tm,
                "file": srcpath + '/' + rows[i][2]
              })
              this.maxtime = Math.max(this.maxtime, tm)
            }
          }
        } else { // multi: Time,Frame,r,Phi,Theta,FILE
          for (i in rows){
              //console.log(rows[i][0])
              if (rows[i][0] && rows[i][5]) {
                tm = parseInt(rows[i][0]) | 0
                this.multifiles.push({
                  "time": tm,
                  "theta": parseInt(rows[i][3]),
                  "phi": parseInt(rows[i][4]),
                  "file": srcpath + '/' + rows[i][5]
                })
                this.maxtime = Math.max(this.maxtime, tm)
              }
            }
        }
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
      var speeds = [1,2,4,8,16,32,64],
          i = 0
      speeds.push(speeds[0]) // terminate
      while (this.speed != speeds[i]) {
        i++
      }
      this.speed = speeds[i+1]
      
      if (this.timeid != null) {
        this.play()
      }
    },
    play()
    {
      if (this.timeid == null) {
        this.timeid = setInterval(()=>{
          var x = parseInt(this.tmslider) + 1
          if (x > this.datum.length - 1) {
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
      if (this.mousedrag) {
        console.log(event.offsetX, event.offsetY)
      }
    },
    imgMouseDown(event)
    {
      this.mousedrag = true
      console.log(event.button)
      event.preventDefault()
    },
    imgMouseUp(event)
    {
      this.mousedrag = false
      console.log(event.button)
     
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
.component {
  margin: 0px;
  max-width: 512px;
  width: 100%;
}
.imagecomp {
  width: 100%;
}
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
.txt {
  margin-left: 5px;
  margin-right: 5px;
  width: 50px;
}
.btn {
  width: 40px;
}
.yoko {
  flex-direction: row;
  display: flex;
  margin-left: 10px;
  margin-right: 10px;
}
</style>
