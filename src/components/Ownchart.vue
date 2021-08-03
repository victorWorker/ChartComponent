<template>
  <div class="Chart_content" style="width: 100%; height: 800px;">
    <div class="buttonBar">
      <button v-for="serie in ownseries" :key="serie.id" @click="setSerie(serie.id)">{{serie.name}}</button>
    </div>
    <div class="tabBar">
      <span><b>{{setSeriedHead.name}} :</b></span>
      <Label for="selectStyle">Style</Label>
      <select id="selectStyle" v-model="setSeriedHead.lineStyle" @change="changeLineType($event)">
        <option value="line">Line</option>
        <option value="area">Area</option>
        <option value="bar">Bars</option>
      </select>
      <input type="color" v-model="setSeriedHead.color" @change="changeColor($event)">
      <Label for="interval" >Interval:</Label>
      <select id="interval" v-model="intervalCall">
        <option value="5">5 min</option>
        <option value="10">10 hours</option>
        <option value="24">1 day</option>
      </select>
      <Label for='axis'>Show axis</Label>
      <input id="axis" type="checkbox" v-model="setSeriedHead.showAxis" @change="changeAxis($event)" checked=ture>
    </div>
    <e-charts autoresize :options="options" theme="ovilia-green" auto-resize />
    <button @click="updateDatas">updatecheck</button>
  </div>
</template>

<script>
import ECharts from 'vue-echarts/components/ECharts'
// import echarts from 'echarts';
import 'echarts'
import 'echarts/lib/component/dataZoom'
import 'echarts/lib/component/grid'
import 'echarts/lib/layout/barGrid'
import 'echarts/lib/component/tooltip'
import 'echarts/lib/component/legend'
import 'echarts/lib/chart/line'
import 'echarts/lib/chart/bar'
import 'echarts/lib/component/axisPointer'
import 'echarts/lib/chart/map'
import 'echarts/lib/echarts'
// import theme from './theme.json'

// ECharts.registerTheme('ovilia-green', theme)

export default {
  props: {
    seriesData: {
      type: Array,
      required: true,
    },
  },
  components: {
    ECharts
  },
  data() {
    return {
      options: {
        tooltip: {
          trigger: 'axis',
        },
        toolbox: {
          feature: {
            dataZoom: {
              yAxisIndex: 'none'
            },
            restore: {},
            saveAsImage: {}
          }
        },
        xAxis: {
          type: 'category',
          data: [],
          scale: true,
          boundaryGap : false,
          axisLine: {onZero: false},
          splitLine: {show: false},
          splitNumber: 100,
          min: 'dataMin',
          max: 'dataMax'
        },
        yAxis: [],
        grid: {
          left: '0%',
          right: '10%',
          bottom: '15%'
        },
        dataZoom: [
          {
            type: 'inside',
            start: 95,
            end: 100
          },
          {
            show: true,
            type: 'slider',
            y: '90%',
            start: 95,
            end: 100,
            handleSize: '80%',
          }
        ],
        series: []
      },
      ownseries: this.seriesData,
      yAxisData: [],
      
      setSeriedHead:{
        id: '',
        name: '',
        lineStyle: '',
        color: '#000',
        showAxis: true,
      },
      intervalCall: 5
    }
  },
  computed: {

  },
  watch:{
    ownseries(value) {
      console.log(value);
      this.options.yAxis = this.getYaxis();
      this.options.series = this.getseries();
    }
  },
  mounted() {
    this.$root.$on('reload', () => {
      console.log(this.seriesData);
    }),
    this.options.yAxis = this.getYaxis();
    this.options.xAxis.data = this.getdates();
    this.options.series = this.getseries();
    this.setSeriedHead.id = this.ownseries[0].id;
    this.setSeriedHead.name = this.ownseries[0].name;
    this.setSeriedHead.lineStyle = this.ownseries[0].settings.type;
    this.setSeriedHead.showAxis = this.ownseries[0].settings.axis;
    this.setSeriedHead.color = this.ownseries[0].settings.color;

  },
  methods: {
    changeColor(event){
      let self = this;
      console.log(event.target.value)
      this.options.series.filter((serie) => {
        if(serie.name === self.setSeriedHead.name){
          serie.itemStyle.color = event.target.value;
          console.log(serie);
        }
      });
    },
    changeAxis(event){
      console.log(event)
      let self = this;
      // alert(this.setSeriedHead.showAxis);
      this.options.yAxis.filter((item) => {
        console.log(item)
        if(item.name === self.setSeriedHead.name){
          item.show = self.setSeriedHead.showAxis;
          // console.log(serie);
        }
      });
    },
    setSerie(id){
      this.ownseries.filter((serie)=>{
        if(serie.id == id){
          this.setSeriedHead.id = serie.id;
          this.setSeriedHead.name = serie.name;
          this.setSeriedHead.lineStyle = serie.settings.type;
          this.setSeriedHead.showAxis = serie.settings.axis;
          this.setSeriedHead.color = serie.settings.color;
        }
      })
    },
    changeLineType(event){
      let self = this;
      this.options.series.filter((serie) => {
        if(serie.name === self.setSeriedHead.name){
          serie.type = event.target.value;
          switch(event.target.value){
            case 'line':
              serie.type = 'line';
              serie.areaStyle.opacity = 0;
              break;
            case 'area':
              serie.type = 'line';
              serie.areaStyle.opacity = 0.65;
              break;
            case 'bar':
              serie.type = 'bar';
              break;
          }
        }
      })

    },
    getdates() {
      return this.ownseries[0].metrics.map((dataitem)=>{
        return dataitem.name;
      })
    },
    getYaxis(){
      this.ownseries.map((item, index) => {
        console.log(Math.max.apply(Math, item.metrics.map((o)=>{return o.value})))
        this.yAxisData.push({
          type: 'value',
          show: true,
          name: item.name,
          min: '0',
          max: 'dataMax',
          scale: true,
          position: 'right',
          offset: 30 * index,
          axisLine: {
            show: true,
            lineStyle: {
              color: item.settings.color
            }
          },
          axisLabel: {
            formatter: '{value}nt',
            color: item.settings.color
          }
        })
      })
      return this.yAxisData;
    },
    getseries() {
      return this.ownseries.map(function (item, index) {
        console.log('wokr', item);
          let datelist = item.metrics.map((dataitem)=>{
            return dataitem.value;
          })
          if(item.settings.type == "area"){
            return {
              type: 'line',
              areaStyle: {
                opacity: 0.65,
              },
              name: item.name,
              yAxisIndex: index,
              showSymbol: false,
              smooth: false,
              itemStyle: {
                color: item.settings.color
              },
              lineStyle: {
                cap: 'round'
              },
              dimentions: ['timestamp', 'name', 'value'],
              encode: {
                x: 'timestamp',
                y: 'value'
              },
              data: datelist
            };
          }else{
            return {
              type: 'line',
              areaStyle: {
                opacity: 0,
              },
              name: item.name,
              yAxisIndex: index,
              showSymbol: true,
              smooth: true,
              itemStyle: {
                color: item.settings.color
              },
              dimentions: ['timestamp', 'name', 'value'],
              encode: {
                x: 'timestamp',
                y: 'value'
              },
              data: datelist
            };
          }
        })
    },
    updateDatas(){
      console.log(this.options);
    }
  },
}
</script>

<style scoped>
.echarts {
  width: 100%;
  height: 100%;
}
.buttonBar{
  width: 100%;
  text-align: left;
  padding: 10px 0;
}
.tabBar{
  z-index: 99999;
  position: relative;
  width: 100%;
  height: 30px;
  margin-bottom: -30px;
  text-align: start;
}
</style>