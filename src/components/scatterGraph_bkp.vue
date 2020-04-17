<template>
    <div
    class="scattergraph"
    ref="container"
    :id='divId'
    :style="{
      height: (height) +  '%',
      width: (width) + '%'
    }"/>
</template>

<script>
import Plotly from 'plotly.js/dist/plotly'
import event from '../event'
export default {
  name: 'scattergraph',
  props: {
    propData: {
      type: Object,
      required: true
    },
    divId: {
      type: String,
      required: true
    },
    height: {
      type: Number,
      default: 100
    },
    width: {
      type: Number,
      default: 95
    }
  },
  data () {
    return {
      // msg: 'Welcome to Your Vue.js App'
    }
  },
  mounted () {
    console.log('divId', this.divId)
    this.newPlot()
  },
  computed: {
    plotDiv () {
      return this.$refs.container
    }
  },
  created () {
    this.$watch('propData', this.update)
    // event.$on('addTrace', function (msg) {
    //   console.log('mesg', msg)
    //   let newTrace = [{
    //     x: [msg[0]],
    //     y: [msg[1]],
    //     z: [msg[2]],
    //     type: 'scatter3d',
    //     mode: 'markers',
    //     showlegend: false,
    //     marker: {size: 10, color: '#C54C82'}
    //   }]
    //   console.log('this.divId', msg[3])
    //   Plotly.addTraces(msg[3], newTrace)
    // }
    // )
    event.$on('animate', function (msg) {
      console.log('animate', msg)
      // this.animate()
      let mydiv = msg[0]
      let data = msg[1]
      console.log('animate data', data[0].x)
      let frames = []
      for (let j = 0; j < data[0].x.length; j++) {
        frames.push({
          data: [{x: [data[0].x[j]], y: [data[0].y[j]], z: [data[0].z[j]]}],
          type: 'scatter3d',
          mode: 'markers',
          marker: {size: 10, color: '#C54C82'}
        })
      }
      console.log('frames', frames)
      // Plotly.addFrames(mydiv, frames)
      Plotly.animate(mydiv, frames, {
        transition: {
          duration: 0
          // easing: 'cubic-in-out'
        },
        frame: {
          duration: 0,
          redraw: false
        },
        mode: 'immediate'
      })
    })
    event.$on('stop', function (msg) {
      console.log('animmate stop')
      Plotly.animate('plot', [], {mode: 'next'})
    })
  },
  methods: {
    update () {
      console.log('update')
      Plotly.react(this.divId, this.propData.data, this.propData.layout, this.propData.config)
    },
    newPlot () {
      Plotly.newPlot(this.divId, this.propData.data, this.propData.layout, this.propData.config)
    }
    // animate () {
    //   console.log('animate', this.divId)
    //   Plotly.animate(this.divId, {
    //     data: [{y: [Math.random(), Math.random(), Math.random()]}]
    //   }, {
    //     transition: {
    //       duration: 500,
    //       easing: 'cubic-in-out'
    //     },
    //     frame: {
    //       duration: 500
    //     }
    //   })
    // }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
