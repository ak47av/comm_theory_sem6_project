<template>
  <div>
    <div class="slidecontainer">
      <input type="range" min="90e+6" max="110e+6" v-model="frequency" class="slider">
      <h1>Frequency : {{ frequency }}</h1>
    </div>
    <div class="slidecontainer">
      <input type="range" min="0" max="500" v-model="modIndex" class="slider">
      <h1>Modulation Index : {{ k }}</h1>
    </div>
    <div id="graph">
    </div>
  </div>
</template>

<script>

import Plotly from '../plotly-latest.min'
import numjs from '../bower_components/numjs/dist/numjs.min'
import {sin, multiply, cos, bignumber} from 'mathjs'

let time = numjs.arange(0,50e-8,1e-10).tolist()
let msg_freq = 10e+6
let msg = sin(multiply(time,msg_freq*6.28))
let integral = multiply(-1,cos(multiply(time,msg_freq*6.28)))

export default {
  name: "FM",
  data(){
    return {
      x: time,
      msg: msg,
      modIndex: 50,
      arr: numjs.zeros(msg.length).tolist(),
      frequency: 92e+6,
      msg_freq: msg_freq,
      integral: integral
    }
  },
  computed:{
    carrier_signal(){
      return sin(multiply(this.x,this.frequency*6.28));
    },
    k(){
      return this.modIndex/100;
    }
  },
  watch:{
    frequency(){
      this.arr = multiply(this.k,this.integral);
      for(let i=0;i<this.msg.length;i++){
        //this.arr[i] = cos((6.28*this.frequency*this.x[i])+(this.k*this.msg));
        this.arr[i] = this.arr[i]+(6.28*this.frequency*this.x[i]);
      }
      this.arr = cos(this.arr);
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
    k(){
      this.arr = multiply(this.k,this.integral);
      for(let i=0;i<this.msg.length;i++){
        //this.arr[i] = cos((6.28*this.frequency*this.x[i])+(this.k*this.msg));
        this.arr[i] = this.arr[i]+(6.28*this.frequency*this.x[i]);
      }
      this.arr = cos(this.arr);
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
    this.arr = multiply(this.k,this.integral);
    for(let i=0;i<this.msg.length;i++){
      //this.arr[i] = cos((6.28*this.frequency*this.x[i])+(this.k*this.msg));
      this.arr[i] = this.arr[i]+(6.28*this.frequency*this.x[i]);
    }
    this.arr = cos(this.arr);
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
