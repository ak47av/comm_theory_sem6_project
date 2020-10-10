<template>
  <div>
    <div id="tabs" class="container">

      <div class="tabs">
        <a v-on:click="plotThis(conventionalPlot)" v-bind:class="[ plots[conventionalPlot] === true ? 'active' : '' ]">Conventional AM</a>
        <a v-on:click="plotThis(dsbscPlot)" v-bind:class="[ plots[dsbscPlot] === true ? 'active' : '' ]">DSB-SC</a>
      </div>

      <div class="content">
        <div v-if="plots[conventionalPlot] === true" class="tabcontent">
          <conventional/>
        </div>
        <div v-if="plots[dsbscPlot] === true" class="tabcontent">
          <DSBSC/>
        </div>
      </div>

    </div>
<!--    <div id="plotters">-->
<!--      <conventional v-if="plots[conventionalPlot]"/>-->
<!--      <DSBSC v-if="plots[dsbscPlot]"/>-->
<!--    </div>-->
  </div>
</template>

<script>
import Conventional from '../components/conventional'
import DSBSC from "@/components/DSBSC";

export default {
name: "amplitude_mod",
  components: {Conventional,DSBSC},
  data(){
    return{
      conventionalPlot: 0,
      dsbscPlot :1,
      plots:[true,false], //[conventional,dsbsc]
    }
  },
  methods:{
    plotThis(plot){
      this.plots=[false,false];
      this.plots[plot] = true;
      return this.plots;
    }
  }

}
</script>

<style scoped>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.container {
  max-width: 90%;
  min-width: 50%;
  min-height: 90%;
  margin: 1px Auto;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 0.9em;
  color: #888;
}

/* Style the tabs */
.tabs {
  overflow: hidden;
  margin-left: 20px;
  margin-bottom: -2px;
}

.tabs ul {
  list-style-type: none;
  margin-left: 20px;
}

.tabs a{
  float: left;
  cursor: pointer;
  padding: 12px 24px;
  transition: background-color 0.2s;
  border: 1px solid #ccc;
  border-right: none;
  background-color: #f1f1f1;
  border-radius: 10px 10px 0 0;
  font-weight: bold;
}
.tabs a:last-child {
  border-right: 1px solid #ccc;
}

/* Change background color of tabs on hover */
.tabs a:hover {
  background-color: #aaa;
  color: #fff;
}

/* Styling for active tab */
.tabs a.active {
  background-color: #fff;
  color: #484848;
  border-bottom: 2px solid #fff;
  cursor: default;
}

/* Style the tab content */
.tabcontent {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 10px;
  box-shadow: 3px 3px 6px #e1e1e1
}
</style>
