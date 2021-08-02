<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <Echarts />
    <button @click="addMetric">Add data</button>
    <Ownchart :seriesData="series"/>
    <Nativechart />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
// import Tradingchart from './components/Tradingchart.vue';
// import Dygraphschart from './components/Dygraphschart.vue';
import Echarts from './components/Echarts.vue';
import Ownchart from './components/Ownchart.vue';
import Nativechart from './components/Nativechart.vue';

import firstdata from './firstdate.json';
import adddata from './adddate.json';

export default {
  name: 'App',
  components: {
    Echarts,
    Ownchart,
    Nativechart,
  },
  date() {
    return {
      onable: true,
      series: []
    }
  },
  computed: {
    series() {
      let datas = [];
      datas.push(firstdata.metrics);
      return datas;
    }
  },
  // mounted() {
  //   console.log(firstdata.metrics);
  //   this.series.push(firstdata.metrics);
  // },
  methods: {
    addMetric(){
      // let metricdata = [];
      // for(var i = 0; i < 30; i++){
      //   console.log(Math.random()*400);
      //   metricdata.push(Math.floor(Math.random()*400));
      // }
      // const addData = {
      //   values: metricdata
      // }
      this.series.push(adddata.metrics);
      console.log(this.series);
      // this.$root.$emit('reload');
    }
  },
  watch: {
    series(newval,oldval) {
      console.log(newval, oldval);
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
