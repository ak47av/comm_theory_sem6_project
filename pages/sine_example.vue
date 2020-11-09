<template>
  <div>
    <div class="slidecontainer">
      <input type="range" min="1" max="100" v-model="frequency" class="slider" id="myRange">
      <h1>Frequency : {{ frequency }}</h1>
    </div>
    <div id="graph">
    </div>
  </div>
</template>

<script>

import Plotly from '../plotly-latest.min'
import numjs from '../numjs.min'
import { sin, multiply, range } from 'mathjs'

let time = numjs.arange(0,10,0.01).tolist()

export default {
  name: "graph",
  data(){
    return{
      x: time,
      frequency: 1
    }
  },
  computed:{
    omega_t(){
      return multiply(this.x,this.frequency*6.28);
    },
    output(){
      return sin(this.omega_t);
    }
  },
  watch:{
     frequency() {
        Plotly.react('graph',[{
          x: this.x,
          y: this.output
        }],{ responsive:true })
    }
  },
  mounted() {
    let time = range(0,10,1);
    Plotly.newPlot('graph', [{
      x: time,
      y: this.output
    }], {responsive:true})
  }
}

</script>

<style>
</style>
