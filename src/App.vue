<template>
  <div id="app">
    <div class="SelectDatas" v-if="!onable">
      <button class="adddata_Btn" @click="addBTCactive()" v-if="addedbtcaddress">Added BTC Metrics</button>
      <button class="adddata_Btn" @click="addBTCactive()" v-else>Add BTC Active Metrics</button>

      <button class="adddata_Btn" @click="addBTCprice()" v-if="addedbtcprice">Added BTC Price Metrics</button>
      <button class="adddata_Btn" @click="addBTCprice()" v-else>Add BTC Price Metrics</button>

      <button class="adddata_Btn" @click="addBTCinflow()" v-if="addedbtcinflow">Added BTC Inflow Metrics</button>
      <button class="adddata_Btn" @click="addBTCinflow()" v-else>Add BTC Inflow Metrics</button>
    </div>
    <button @click="startChart" v-if="!onable">Start</button>
    
    <Ownchart :seriesData="series" v-if="onable"/>
  </div>
</template>

<script>
// import axios from 'axios';

import Ownchart from './components/Ownchart.vue';

import addressdata from './firstdate.json';
import pricedata from './adddate.json';
import infodata from './infodate.json';

export default {
  name: 'App',
  components: {
    Ownchart,
  },
  data() {
    return {
      onable: false,
      addedbtcaddress: false,
      addedbtcprice: false,
      addedbtcinflow: false,
      series: [],
      image: 'https://echarts.apache.org/examples/data/asset/geo/ksia-ext-plan-min.svg'
      // addressUrl: 'http://api.glassnode.com/v1/metrics/addresses/active_count?a=BTC&i=24h&api_key=1wF1lvgNID8PBYUQXeVE1Ono1Dp',
      // priceUrl: 'http://api.glassnode.com/v1/metrics/market/price_usd_close?a=BTC&i=24h&api_key=1wF1lvgNID8PBYUQXeVE1Ono1Dp',
      // inflowUrl: 'https://api.glassnode.com/v1/metrics/transactions/transfers_volume_to_exchanges_sum?a=BTC&c=native&e=aggregated&i=24h&api_key=1wF1lvgNID8PBYUQXeVE1Ono1Dp',
    }
  },
  computed: {
    // series() {
    //   let datas = [];
    //   datas.push(firstdata);
    //   return datas;
    // }
  },
  mounted() {
  },
  methods: {
    addBTCactive(){
      this.addedbtcaddress = !this.addedbtcaddress;
      console.log(this.addedbtcaddress)
    },
    addBTCprice(){
      this.addedbtcprice = !this.addedbtcprice;
    },
    addBTCinflow(){
      this.addedbtcinflow = !this.addedbtcinflow;
    },
    // addMetric(){
    //   this.series.push(adddata);
    //   console.log(this.series);
    // }
    startChart() {
      // axios.defaults.headers.common['Access-Control-Allow-Origin'] = '*';
      if(this.addedbtcaddress || this.addedbtcprice || this.addedbtcinflow){
        if(this.addedbtcinflow){
          // const headers = {};
          // axios.get(this.inflowUrl, headers).then((res)=>{
          //   console.log(res);
          // })
          const addedinflowdata = {
            id: 'inflow',
            name: 'inflow',
            settings: {
              type: 'area',
              color: '#f00',
              axis: true,
            },
            metrics: infodata,
          };
          this.series.push(addedinflowdata);
        }
        if(this.addedbtcaddress) {
          const addedaddressdata = {
            id: 'BTC address',
            name: 'BTC address',
            settings: {
              type: 'line',
              color: '#0f0',
              axis: true
            },
            metrics: addressdata,
          };
          this.series.push(addedaddressdata);
        }
        if(this.addedbtcprice) {
          const addedpricedata = {
            id: 'BTC Price',
            name: 'BTC Price',
            settings: {
              type: 'bar',
              color: '#00f',
              axis: true,
            },
            metrics: pricedata
          };
          this.series.push(addedpricedata);
        }
        this.onable = true;  
      }else{
        alert('Please select the Metrics data');
      }
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
.SelectDatas{
  padding: 10px 15px;
}
.adddata_Btn{
  margin: 4px;
}
</style>
