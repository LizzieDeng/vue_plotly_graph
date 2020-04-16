<template>
  <div id="app">
<!--    <img src="./assets/logo.png">-->
<!--    <router-view/>-->
    <div id="contain">
    <el-row>
      <span>定位坐标点:</span>
      <el-select v-model="value" placeholder="请选择" @change="selectStepGet">
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value">
        </el-option>
      </el-select>
        <el-button type="primary" plain @click.native="refreshGraph">刷新散点图</el-button>
      </el-row>
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
    // eslint-disable-next-line camelcase,no-undef
    // let listY = []
    // let listZ = []
    // for (let i = 0; i < 15; i++) {
    //   let rand1 = parseInt(Math.random() * 100)
    //   let rand2 = parseInt(Math.random() * 50)
    //   listY.push(rand1)
    //   listZ.push(rand2)
    // }
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
    this.getOptions()
  },
  methods: {
    updatePlot () {
      console.log('updatePlot')
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
    },
    // 获取选中步伐
    selectStepGet (step) {
      let obj = this.options.find((item) => {
        console.log('step', step)
        if (item.value === step) {
          this.step = parseInt(step)
        }
        return item.value === step
      })
      console.log('obj', obj.value, this.step)

      // get the step point data and add trace
      // eslint-disable-next-line camelcase
      let newX, newY, newZ
      newX = this.plotInfo.data[0].x[this.step]
      newY = this.plotInfo.data[0].y[this.step]
      newZ = this.plotInfo.data[0].z[this.step]
      let divId = 'plot'
      console.log('emit', newX, newY, newZ, divId)
      event.$emit('addTrace', [newX, newY, newZ, divId])
    },
    getOptions () {
      for (let i = 0; i < this.plotInfo.data[0].y.length; i++) {
        let dic = {}
        // 定义一个字典
        dic.value = i
        dic.label = i.toString()
        this.options.push(dic)
      }
      console.log('options', this.options)
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
  margin-top: 10px;
}
#contain {
  /*float: left;*/
  margin-bottom: 10px;
  margin-left: 50px;
}
#time{
   /*margin-top: 10px;*/
}
</style>
