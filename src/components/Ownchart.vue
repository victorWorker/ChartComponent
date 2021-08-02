<template>
  <div class="Chart_content" style="width: 100%; height: 800px;">
    <e-charts autoresize :options="options" theme="ovilia-green" ref="line" auto-resize />
    <button @click="updateDatas">updatecheck</button>
  </div>
</template>

<script>
import ECharts from 'vue-echarts/components/ECharts'
// import echarts from 'echarts';
import 'echarts/lib/component/tooltip'
import 'echarts/lib/component/legend'
import 'echarts/lib/chart/line'
import 'echarts/lib/chart/bar'
import 'echarts/lib/chart/map'
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
          trigger: 'axis'
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
        },
        yAxis: [{
          splitLine: { show: false }
        },
        {
          type: 'value',
          splitLine: { show: false }
        }],
        grid: {
          left: '10%',
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
            Z:2
          }
        ],
        series: []
      },
      ownseries: this.seriesData
    }
  },
  computed: {

  },
  watch:{
    ownseries(value) {
      console.log(value);
      this.options.series = this.getseries();
    }
  },
  mounted() {
    this.$root.$on('reload', () => {
      console.log(this.seriesData);
    }),
    this.options.xAxis.data = this.getdates();
    this.options.series = this.getseries();
  },
  methods: {
    getdates() {
      return this.ownseries[0].map((dataitem)=>{
        return dataitem.name;
      })
    },
    getseries() {
      return this.ownseries.map(function (item) {
        console.log('wokr', item);
          let datelist = item.map((dataitem)=>{
            return dataitem.value;
          })
          return {
            type: 'line',
            showSymbol: true,
            smooth: true,
            dimentions: ['timestamp', 'name', 'value'],
            encode: {
              x: 'timestamp',
              y: 'value'
            },
            data: datelist
          };
        })
    },
    updateDatas(){
      console.log(this.seriesData);
    }
  },
}
</script>

<style scoped>
.echarts {
  width: 100%;
  height: 100%;
}
</style>