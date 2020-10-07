<template>
  <div id="graph">

  </div>
</template>

<script>

import Plotly from '../plotly-latest.min'

export default {
  name: "graph",
  mounted() {
    let externalScript = document.createElement('script');
    externalScript.setAttribute('src','https://cdn.plot.ly/plotly-latest.min.js')
    document.head.appendChild(externalScript);

    Plotly.newPlot('graph', [{
      x: [1, 2, 3],
      y: [2, 1, 3]
    }], {
      sliders: [{
        pad: {t: 30},
        currentvalue: {
          xanchor: 'right',
          prefix: 'color: ',
          font: {
            color: '#888',
            size: 20
          }
        },
        steps: [{
          label: 'red',
          method: 'restyle',
          args: ['line.color', 'red']
        }, {
          label: 'green',
          method: 'restyle',
          args: ['line.color', 'green']
        }, {
          label: 'blue',
          method: 'restyle',
          args: ['line.color', 'blue']
        }]
      }]
    })
  }
}

</script>

<style>
</style>
