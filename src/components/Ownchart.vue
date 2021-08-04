<template>
  <div class="Chart_content" style="width: 100%; height: 800px;">
    <div class="buttonBar">
      <span class="seriesButton" v-for="serie in ownseries" :key="serie.id" @click="setSerie(serie.id)">
        {{serie.name}}
        <!-- <v-icon name="more-vertical"></v-icon>
        <span @click="closeSerie(serie.name)">
          <v-icon name="x"></v-icon>
        </span> -->
      </span>
    </div>
    <div class="tabBar">
      <span><b>{{setSeriedHead.name}} :</b></span>
      <span class="controllSpan">
        <Label for="selectStyle">Style</Label>
        <select id="selectStyle" class="selectStyle" v-model="setSeriedHead.lineStyle" @change="changeLineType($event)">
          <option value="line">Line</option>
          <option value="area">Area</option>
          <option value="bar">Bars</option>
        </select>
      </span>
      <span class="controllSpan">
        <input type="color" class="selectLayerColor" v-model="setSeriedHead.color" @change="changeColor($event)" >
      </span>
      <span class="controllSpan">
        <Label for="interval" >Interval:</Label>
        <select id="interval" class="selectInterval" v-model="intervalCall">
          <option value="5">5 min</option>
          <option value="10">10 hours</option>
          <option value="24">1 day</option>
        </select>
      </span>
      <span class="controllSpan">
        <Label for='axis'>Show axis</Label>
        <input id="axis" class="selectShowAxis" type="checkbox" v-model="setSeriedHead.showAxis" @change="changeAxis($event)" checked=ture>
      </span>
    </div>
    <e-charts ref="chartmap" autoresize :options="options" theme="ovilia-green" auto-resize />
  </div>
</template>

<script>
import ECharts from 'vue-echarts/components/ECharts'
// import echarts from 'echarts';
import 'echarts'
import 'echarts/lib/component/dataZoom'
import 'echarts/lib/component/grid'
import 'echarts/lib/component/tooltip'
import 'echarts/lib/component/legend'
import 'echarts/lib/chart/line'
import 'echarts/lib/chart/bar'
import 'echarts/lib/component/axisPointer'
import 'echarts/lib/chart/map'
import 'echarts/lib/echarts'

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
    // this.$refs.chartmap.echarts.registerMap('map', {svg: 'eeeee'});
    return {
      options: {
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross',
            label: {
              backgroundColor: '#6a7985'
            }
          }
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
        visualMap: {
          show: true,
          inRange: {
              color: ['#d94e5d','#eac736','#50a3ba']
          },
          outOfRange: {
              color: ['#ccc'] //['#d94e5d','#eac736','#50a3ba']
          },
          top: 20,
          textStyle: {
              color: '#fff'
          },
          realtime: false
        },
        calculable: true,
        legend: {
          data: [],
          itemGap: 5,
        },
        xAxis: {
          type: 'category',
          data: [],
          scale: true,
          boundaryGap : false,
          axisLine: {onZero: false},
          splitLine: {show: true},
          splitNumber: 100,
          min: 'dataMin',
          max: 'dataMax'
        },
        yAxis: [{}],
        grid: {
          left: '0%',
          right: '10%',
          bottom: '15%',
          containLabel: true,
          backgroundColor: '#333',
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
      legendData: [],
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
      console.log(this.getYaxis());
      this.options.yAxis = this.getYaxis();
      this.options.series = this.getseries();
      this.options.legend.data = this.legendData;
      console.log(this.options);
    }
  },
  mounted() {
    this.$root.$on('reload', () => {
      console.log(this.seriesData);
    }),
    this.options.yAxis = this.getYaxis();
    this.options.xAxis.data = this.getdates();
    this.options.series = this.getseries();
    this.options.legend.data = this.legendData;
    this.setSeriedHead.id = this.ownseries[0].id;
    this.setSeriedHead.name = this.ownseries[0].name;
    this.setSeriedHead.lineStyle = this.ownseries[0].settings.type;
    this.setSeriedHead.showAxis = this.ownseries[0].settings.axis;
    this.setSeriedHead.color = this.ownseries[0].settings.color;
    console.log('test', this.setSeriedHead);
  },
  created(){
    this.setSeriedHead.id = this.ownseries[0].id;
    this.setSeriedHead.name = this.ownseries[0].name;
    this.setSeriedHead.lineStyle = this.ownseries[0].settings.type;
    this.setSeriedHead.showAxis = this.ownseries[0].settings.axis;
    this.setSeriedHead.color = this.ownseries[0].settings.color;
  },
  methods: {
    changeColor(event){
      let self = this;
      this.options.series.filter((serie) => {
        if(serie.name === self.setSeriedHead.name){
          serie.itemStyle.color = event.target.value;
        }
      });
    },
    changeAxis(event){
      let self = this;
      console.log(event.target);
      this.options.yAxis.filter((item) => {
        if(item.name === self.setSeriedHead.name){
          item.show = self.setSeriedHead.showAxis;
        }
      });
    },
    closeSerie(id){
      alert(id);
      this.ownseries = this.ownseries.filter((serie) => {
        if(serie.name == id){
          return serie;
        }
      })
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
        // new Date(dataitem.t);
        return new Date(dataitem.t).toLocaleDateString();
      })
    },
    getYaxis(){
      const yAxisData = [];
      this.ownseries.map((item, index) => {
        console.log(Math.max.apply(Math, item.metrics.map((o)=>{return o.v})))
        yAxisData.push({
          type: 'value',
          show: true,
          name: item.name,
          min: '0',
          max: 'dataMax',
          scale: true,
          position: 'right',
          offset: 80 * index,
          axisLine: {
            show: true,
            lineStyle: {
              color: item.settings.color
            }
          },
          axisLabel: {
            formatter: '{value}',
            color: item.settings.color
          }
        })
      })
      return yAxisData;
    },
    getseries() {
      let self = this;
      self.legendData = [];
      return this.ownseries.map(function (item, index) {
        let datelist = item.metrics.map((dataitem)=>{
          return dataitem.v;
        })
        self.legendData.push(item.name);
        let returnvalue={};
        switch(item.settings.type){
          case 'area':
            returnvalue = {
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
              emphasis: {
                focus: 'series'
              },
              dimentions: ['t', 'v'],
              encode: {
                x: 't',
                y: 'v'
              },
              data: datelist
            };
            break;
          case 'line':
            returnvalue = {
              type: 'line',
              areaStyle: {
                opacity: 0,
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
              emphasis: {
                focus: 'series'
              },
              dimentions: ['t', 'v'],
              encode: {
                x: 't',
                y: 'v'
              },
              data: datelist
            };
            break;
          case 'bar':
            returnvalue = {
              type: 'bar',
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
              emphasis: {
                focus: 'series'
              },
              dimentions: ['t', 'v'],
              encode: {
                x: 't',
                y: 'v'
              },
              data: datelist
            };
            break;
        }
        return returnvalue;
      })
    },
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
  margin-bottom: 0px;
  text-align: start;
}
.controllSpan{
  margin: 3px 6px;
  border-bottom: 1px solid;
  display: inline-flex;
  justify-content: center;
  align-items: center;
}
.selectStyle {
  border: none;
  font-size: 15px;
}
.selectLayerColor{
  width: 25px;
  height: 20px;
}
.selectInterval{
  border: none;
  font-size: 15px;
}
.selectShowAxis{
    width: 14px;
    height: 14px;
    margin: 0 3px !important;
    padding: 0 !important;
}
.seriesButton{
  padding: 10px;
  border-radius: 5px;
  margin: 3px;
  user-select: none;
  border: 1px solid #000;
  width: auto;
  position: relative;
  display: inline-flex;
  justify-content: center;
  align-items: center;
}
.icon{
   width: 24px;
}
</style>