<template>
  <div>
    <div class="slidecontainer">
      <input type="range" min="550e+3" max="1720e+3" v-model="frequency" class="slider">
      <h1>Frequency : {{ frequency }}</h1>
    </div>
    <div class="slidecontainer">
      <input type="range" min="0" max="100" v-model="modIndex" class="slider">
      <h1>Modulation Index : {{ mu }}</h1>
    </div>
    <div id="graph">
    </div>
  </div>
</template>

<script>

import Plotly from '../plotly-latest.min'
import numjs from '../bower_components/numjs/dist/numjs.min'
import {sin, multiply} from 'mathjs'

let time = numjs.arange(0,25e-6,0.01e-7).tolist()
let msg_freq = 10e+4
let msg = sin(multiply(time,msg_freq*6.28))

export default {
  name: "conventional AM",
  data(){
    return {
      x: time,
      msg: msg,
      modIndex: 50,
      arr: numjs.zeros(msg.length).tolist(),
      frequency: 550e+3,
      msg_freq: msg_freq
    }
  },
  computed:{
    carrier_signal(){
      return sin(multiply(this.x,this.frequency*6.28));
    },
    mu(){
      return this.modIndex/100;
    }
  },
  watch:{
    frequency(){
      for(let i=0;i<this.msg.length;i++){
        this.arr[i] = this.carrier_signal[i]*(1+(this.mu*this.msg[i]));
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
    },
    mu(){
      for(let i=0;i<this.msg.length;i++){
        this.arr[i] = this.carrier_signal[i]*(1+(this.mu*this.msg[i]));
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
      this.frequency = this.frequency++;
    }
  },
  mounted(){
    for(let i=0;i<this.msg.length;i++){
      this.arr[i] = this.carrier_signal[i]*(1+this.mu*this.msg[i]);
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
