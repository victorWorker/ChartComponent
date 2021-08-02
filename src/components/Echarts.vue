<template>

<div class="chart_content" style="width: 1000px; height: 500px;">
  <e-charts autoresize :options="line" theme="ovilia-green" ref="line" auto-resize />
</div>
</template>

<style scoped>
/* .chart_content{
  width: '1000px';
  height: '500px';
} */
.echarts {
    width: 100%;
    height: 100%;
}
</style>

<script>
import ECharts from 'vue-echarts/components/ECharts'
import 'echarts/lib/component/tooltip'
import 'echarts/lib/component/legend'
import 'echarts/lib/chart/line'
import 'echarts/lib/chart/bar'
import 'echarts/lib/chart/map'
import theme from './theme.json'

ECharts.registerTheme('ovilia-green', theme)

export default {
  data () {
    const data = [
      ['2000-06-05', 116, 30],
      ['2000-06-06', 129, 100],
      ['2000-06-07', 135, 322],
      ['2000-06-08', 86, 85],
      ['2000-06-09', 73, 120],
      ['2000-06-10', 85, 23],
      ['2000-06-11', 73, 85],
      ['2000-06-12', 68, 85],
      ['2000-06-13', 92, 85],
      ['2000-06-14', 130, 85],
      ['2000-06-15', 245, 85],
      ['2000-06-16', 139, 85],
      ['2000-06-17', 115, 85],
      ['2000-06-18', 111, 85],
      ['2000-06-19', 309, 85],
      ['2000-06-20', 206, 85],
      ['2000-06-21', 137, 206],
      ['2000-06-22', 128, 206],
      ['2000-06-23', 85, 206],
      ['2000-06-24', 94, 206],
      ['2000-06-25', 71, 206],
      ['2000-06-26', 106, 206],
      ['2000-06-27', 84, 206],
      ['2000-06-28', 93, 206],
      ['2000-06-29', 85, 206],
      ['2000-06-30', 73, 206],
      ['2000-07-01', 83, 206],
      ['2000-07-02', 125, 125],
      ['2000-07-03', 107, 125],
      ['2000-07-04', 82, 125],
      ['2000-07-05', 44, 125],
      ['2000-07-06', 72, 125],
      ['2000-07-07', 106, 125],
      ['2000-07-08', 107, 125],
      ['2000-07-09', 66, 125],
      ['2000-07-10', 91, 125],
      ['2000-07-11', 92, 91],
      ['2000-07-12', 113, 91],
      ['2000-07-13', 107, 91],
      ['2000-07-14', 131, 91],
      ['2000-07-15', 111, 91],
      ['2000-07-16', 64, 91],
      ['2000-07-17', 69, 91],
      ['2000-07-18', 88, 69],
      ['2000-07-19', 77, 69],
      ['2000-07-20', 83, 69],
      ['2000-07-21', 111, 69],
      ['2000-07-22', 57, 69],
      ['2000-07-23', 55, 69],
      ['2000-07-24', 60, 69]
    ]
    const dateList = data.map(function (item) {
      return item[0]
    })
    const valueList = data.map(function (item) {
      return item[1]
    })
    const olderList = data.map(function (item) {
      return item[2]
    })

    return {
     data: [
      ['2000-06-05', 116, 30],
      ['2000-06-06', 129, 100],
      ['2000-06-07', 135, 322],
      ['2000-06-08', 86, 85],
      ['2000-06-09', 73, 120],
      ['2000-06-10', 85, 23],
      ['2000-06-11', 73, 85],
      ['2000-06-12', 68, 85],
      ['2000-06-13', 92, 85],
      ['2000-06-14', 130, 85],
      ['2000-06-15', 245, 85],
      ['2000-06-16', 139, 85],
      ['2000-06-17', 115, 85],
      ['2000-06-18', 111, 85],
      ['2000-06-19', 309, 85],
      ['2000-06-20', 206, 85],
      ['2000-06-21', 137, 206],
      ['2000-06-22', 128, 206],
      ['2000-06-23', 85, 206],
      ['2000-06-24', 94, 206],
      ['2000-06-25', 71, 206],
      ['2000-06-26', 106, 206],
      ['2000-06-27', 84, 206],
      ['2000-06-28', 93, 206],
      ['2000-06-29', 85, 206],
      ['2000-06-30', 73, 206],
      ['2000-07-01', 83, 206],
      ['2000-07-02', 125, 125],
      ['2000-07-03', 107, 125],
      ['2000-07-04', 82, 125],
      ['2000-07-05', 44, 125],
      ['2000-07-06', 72, 125],
      ['2000-07-07', 106, 125],
      ['2000-07-08', 107, 125],
      ['2000-07-09', 66, 125],
      ['2000-07-10', 91, 125],
      ['2000-07-11', 92, 91],
      ['2000-07-12', 113, 91],
      ['2000-07-13', 107, 91],
      ['2000-07-14', 131, 91],
      ['2000-07-15', 111, 91],
      ['2000-07-16', 64, 91],
      ['2000-07-17', 69, 91],
      ['2000-07-18', 88, 69],
      ['2000-07-19', 77, 69],
      ['2000-07-20', 83, 69],
      ['2000-07-21', 111, 69],
      ['2000-07-22', 57, 69],
      ['2000-07-23', 55, 69],
      ['2000-07-24', 60, 69]
    ],
      seriesData: [
        {
          type: 'line',
          values: valueList,
        },
        {
          type: 'bar',
          values: olderList,
        }
      ],
      line: {
        visualMap: [
          {
            show: false,
            type: 'continuous',
            min: 0,
            max: 400
          }
        ],
        tooltip: {
          trigger: 'axis'
        },
        xAxis: {
          data: dateList,
        },
        yAxis: {
          splitLine: { show: false }
        },
        series:[ {
          type: 'line',
          name: 'BET',
          showSymbol: false,
          data: valueList
        },
           {
          type: 'line',
          name: 'TCD',
          showSymbol: false,
          areaStyle: {},
          data: olderList
        }
        ]
      }
    }
  },
  computed: {
    realDatas() {
      const gettedseries = this.seriesData.map(function (item) {
        return {
          type: item.type,
          showSymbol: false,
          data: item.values,
        }
      })
      const datalist = this.data.map(function (item) {
      return item[0]
    })
      return  {
        visualMap: [
          {
            show: false,
            type: 'continuous',
            min: 0,
            max: 400
          }
        ],
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
        dataZoom: [{
              type: 'inside',
              start: 0,
              end: 20
          }, {
              start: 0,
              end: 20
          }],
        xAxis: {
          data: datalist
        },
        yAxis: {
          splitLine: { show: false }
        },
        series: gettedseries
      }
    }
  },
  components: {
    ECharts
  }
}
</script>