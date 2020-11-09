<template>
  <div>
    <div class="slidecontainer">
      <input type="range" min="550e+3" max="1720e+3" v-model="frequency" class="slider" id="myRange">
      <h1>Frequency : {{ frequency }}</h1>
    </div>
    <div id="graph">
    </div>
  </div>
</template>

<script>

import Plotly from '../plotly-latest.min'
import numjs from '../numjs.min'
import {sin, multiply} from 'mathjs'

//let carrier_freqs = numjs.arange(550e+3,1720e+3,100).tolist()
let time = numjs.arange(0,25e-6,0.01e-7).tolist()
let msg_freq = 1e+5
let msg = sin(multiply(time,msg_freq*6.28))

export default {
  name: "conventional AM",
  data(){
    return {
      x: time,
      msg: msg,
      Ac: 1,
      Am: 1,
      arr: numjs.zeros(msg.length).tolist(),
      frequency: 550e+3,
      msg_freq: msg_freq
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
        title: "f" + "m".sub() + " = " + this.msg_freq.toExponential() + " Hz",
        grid: {rows: 1, columns: 3, pattern: 'independent'},
      });
    }
  },
  mounted(){
    for(let i=0;i<this.msg.length;i++){
      this.arr[i] = this.Ac*this.Am*this.carrier_signal[i]*this.msg[i];
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
      title: "f" + "m".sub() + " = " + this.msg_freq.toExponential() + " Hz",
      grid: {rows: 1, columns: 3, pattern: 'independent'},
    });
  }
}
</script>

<style scoped>

</style>
