<template>
  <D3BarChart :config="config" :datum="chartData" />
</template>

<script>
import { D3BarChart } from 'vue-d3-charts';
export default {
  components: {
    D3BarChart
  },
  data() {
    return {
      chartData: [
        {name: 'lorem', total: 30},
        {name: 'kingl', total: 10},
        {name: 'werd', total: 20},
        {name: 'ijs', total: 35},
        {name: 'wq', total: 45},
      ],
      config: {
        key: "name",
        value: "total",
        color: "steelblue",
      }
    }
  }
}
</script>
