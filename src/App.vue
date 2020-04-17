<template>
  <div id="app">
    <div id="contain">
        <el-button type="primary" plain @click.native="refreshGraph">刷新散点图</el-button>
    </div>
     <scatterGraph :propData="plotInfo" divId="plot"  style="height: 800px"></scatterGraph>
     <div id="time"> {{status}}</div>
  </div>
</template>

<script>
import scatterGraph from './components/scatterGraph'
export default {
  name: 'App',
  components: {scatterGraph},
  data: function () {
    return ({
      plotInfo: {
        data: [{
          x: [30, 50, 20, 60, 44, 88, 98, 22],
          y: [30, 50, 20, 60, 44, 88, 70, 22],
          z: [30, 50, 20, 10, 44, 19, 9, 22],
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
      let data = [{ x: [0],
        y: [0],
        z: [0],
        name: '动画',
        type: 'scatter3d',
        mode: 'markers',
        marker: {size: 0},
        opacity: 0
      }, {
        x: x,
        y: y,
        z: z,
        type: 'scatter3d',
        name: '散点',
        mode: 'markers',
        opacity: 0.5
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
        scene: {
          aspectmode: 'data',
          xaxis: {
            range: [0, 100]
          },
          yaxis: {
            range: [0, 100]
          },
          zaxis: {
            range: [0, 100]
          }
        },
        updatemenus: [{
          type: 'buttons',
          active: 0,
          buttons: [
            {label: '开始',
              method: 'animate',
              args: [null,
                {frame: {duration: 200, redraw: true},
                  fromcurrent: true,
                  mode: 'immediate',
                  transition: {duration: 300, easing: 'quadratic-in-out'}}]
            },
            {label: '暂停',
              method: 'animate',
              args: [[null],
                {frame: {duration: 0, redraw: false},
                  mode: 'immediate',
                  transition: {duration: 0}}]
            }
          ]
        }],
        paper_bgcolor: '#ECEFF1'
      }
      let config = {
        // newish in plotly...auto-resizes on window resize
        responsive: true
      }
      this.plotInfo = {data: data, layout: layout, config: config}
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
