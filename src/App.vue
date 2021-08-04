<template>
  <div id="app">
    <button @click="addMetric">Add data</button>
    <Ownchart :seriesData="series"/>
  </div>
</template>

<script>
import Ownchart from './components/Ownchart.vue';

import firstdata from './firstdate.json';
import adddata from './adddate.json';

export default {
  name: 'App',
  components: {
    Ownchart,
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
      datas.push(firstdata);
      return datas;
    }
  },
  mounted() {
    
  },
  methods: {
    addMetric(){
      this.series.push(adddata);
      console.log(this.series);
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
