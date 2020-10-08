<template>
<div>
  <div class="slidecontainer">
    <input type="range" min="550e+3" max="1720e+3" v-model="frequency" class="slider" id="myRange">
    <h1>Frequency : {{ frequency }}</h1>
    <h4>{{typeof (x)}}{{typeof(msg)}}</h4>
  </div>
  <div id="graph">
  </div>
</div>
</template>

<script>

import Plotly from '../plotly-latest.min'
import numjs from '../bower_components/numjs/dist/numjs.min'
import {sin, multiply, cos, bignumber} from 'mathjs'

let carrier_freqs = numjs.arange(550e+3,1720e+3,100).tolist()
let time = numjs.arange(0.01e-6,50e-6,0.01e-7).tolist()
let msg = sin(multiply(time,100000*6.28))

export default {
name: "amplitude_mod",
  data(){
    return {
      x: time,
      msg: msg,
      arr: numjs.zeros(msg.length).tolist(),
      frequency: 550e+3
    }
  },
  computed:{
    carrier_signal(){
      return sin(multiply(this.x,this.frequency*6.28));
    }
  },
  watch:{
    frequency(){
      for(let i=0;i<this.msg.length;i++){
        this.arr[i] = this.carrier_signal[i]*this.msg[i];
      }
      Plotly.react('graph', [{
        x: this.x,
        y: this.carrier_signal
      },{
        x:this.x,
        y:this.msg,
        xaxis:'x2',
        yaxis:'y2'
      },{
        x:this.x,
        y:this.arr,
        xaxis:'x3',
        yaxis:'y3'
      }],{
        grid: {rows: 1, columns: 3, pattern: 'independent'},
      });
    }
  },
  mounted(){
    for(let i=0;i<this.msg.length;i++){
      this.arr[i] = this.carrier_signal[i]*this.msg[i];
    }
    Plotly.newPlot('graph', [{
      x: this.x,
      y: this.carrier_signal
    },{
      x:this.x,
      y:this.msg,
      xaxis:'x2',
      yaxis:'y2'
    },{
      x:this.x,
      y:this.arr,
      xaxis:'x3',
      yaxis:'y3'
    }],{
      grid: {rows: 1, columns: 3, pattern: 'independent'},
    });
  }
}
</script>

<style scoped>

</style>
