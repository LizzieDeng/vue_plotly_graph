<template>
  <div id="app">
<!--    <img src="./assets/logo.png">-->
<!--    <router-view/>-->
    <div id="contain">
        <el-button type="primary" plain @click.native="refreshGraph" style="float: left">刷新散点图</el-button>
        <el-button type="primary" plain @click.native="playAnimate"  style="margin-left: 10px; margin-right: 10px">播放</el-button>
        <el-button type="primary" plain @click.native="stopAnimate" >暂停</el-button>
    </div>
     <scattergraph :propData="plotInfo" divId="plot"  style="height: 800px"></scattergraph>
     <div id="time"> {{status}}</div>
  </div>
</template>

<script>
import Scattergraph from './components/scatterGraph'
import event from './event'

export default {
  name: 'App',
  components: {Scattergraph},
  data: function () {
    return ({
      plotInfo: {
        data: [{
          x: [0],
          y: [0],
          z: [0],
          type: 'scatter3d',
          mode: 'markers',
          showlegend: false
        }]
      },
      step: '',
      options: [],
      value: '',
      status: ''
    })
  },
  mounted () {
    this.updatePlot()
    // this.getOptions()
  },
  methods: {
    updatePlot () {
      let x = []
      let y = []
      let z = []
      for (let i = 0; i < 15; i++) {
        let rand1 = parseInt(Math.random() * 100)
        let rand2 = parseInt(Math.random() * 80)
        let rand3 = parseInt(Math.random() * 50)
        x.push(rand1)
        y.push(rand2)
        z.push(rand3)
      }
      let data = [{
        x: x,
        y: y,
        z: z,
        type: 'scatter3d',
        mode: 'markers',
        showlegend: false
      }]
      let layout = {
        height: 700,
        width: 800,
        // get rid of all the extra whitespace around plots
        margin: {
          t: 25,
          b: 25,
          r: 25,
          l: 25
        },
        paper_bgcolor: '#ECEFF1'
      }
      let config = {
        // newish in plotly...auto-resizes on window resize
        responsive: true
      }
      this.plotInfo = {data: data, layout: layout, config: config}
      console.log('updatePlot', this.plotInfo.data[0].x)
    },
    playAnimate () {
      let divId = 'plot'
      console.log('emit animate', this.plotInfo.data[0].x)
      event.$emit('animate', [divId, this.plotInfo.data])
    },
    stopAnimate () {
      event.$emit('stop')
    },
    refreshGraph () {
      this.updatePlot()
      this.status = 'Plot last updated at ' + new Date()
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.scattergraph {
  margin-top: 60px;
}
#contain {
  /*float: left;*/
  margin-bottom: 10px;
  /*margin-left: 50px;*/
}
#time{
   /*margin-top: 10px;*/
}
</style>
