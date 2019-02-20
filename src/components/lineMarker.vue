<template>
  <div :class="className" :id="id" :style="{height:height,width:width}"/>
</template>

<script>
import echarts from 'echarts'
import resize from './mixins/resize'

export default {
  mixins: [resize],
  props: {
    className: {
      type: String,
      default: 'chart'
    },
    id: {
      type: String,
      default: 'chart'
    },
    width: {
      type: String,
      default: '200px'
    },
    height: {
      type: String,
      default: '200px'
    },
    title: {
      type: String,
      default: 'Temperatura'
    },
    bgcolor: {
      type: String,
      default: 'black'
    },
    contentcolor: {
      type: String,
      default: 'white'
    }
  },
  data() {
    return {
      chart: null,
      weather_history_dates: [],
      weather_history_temps: []
    }
  },
  mounted() {
    this.initChart()
  },
  beforeDestroy() {
    if (!this.chart) {
      return
    }
    this.chart.dispose()
    this.chart = null
  },
  methods: {
    fetchData() {
      fetch('https://weather-api-01.herokuapp.com/getweather')
            .then(res => res.json())
            .then(data => {
              // eslint-disable-next-line
              //console.log(data);

              data.forEach(el => {
                this.weather_history_dates.push(el.date);
                this.weather_history_temps.push(el.temp);
              })

                this.chart.setOption({
                                      xAxis: [{
                                        data: this.weather_history_dates
                                      }
                                      ],
                                      series: [
                                        {
                                          data: this.weather_history_temps
                                        }
                                      ]
                                    });
            });
    },
    initChart() {

      this.chart = echarts.init(document.getElementById(this.id))

      this.chart.setOption({
        backgroundColor: this.bgcolor,
        title: {
          top: 20,
          text: this.title,
          textStyle: {
            fontWeight: 'normal',
            fontSize: 16,
            color: this.contentcolor
          },
          left: '1%'
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            lineStyle: {
              color: 'rgba(194, 239, 179, 0.28)'
            }
          }
        },
        legend: {
          top: 20,
          icon: 'circle',
          itemWidth: 14,
          itemHeight: 14,
          itemGap: 13,
          data: ['Temperatura'],
          right: '4%',
          textStyle: {
            fontSize: 12,
            color: this.contentcolor
          }
        },
        grid: {
          top: 100,
          left: '2%',
          right: '2%',
          bottom: '2%',
          containLabel: true
        },
        xAxis: [{
          type: 'category',
          boundaryGap: false,
          axisLine: {
            lineStyle: {
              color: this.contentcolor
            }
          },
          data: []
        }],
        yAxis: [{
          type: 'value',
          name: '(\u00B0C)',
          axisTick: {
            show: false
          },
          axisLine: {
            lineStyle: {
              color: this.contentcolor
            }
          },
          axisLabel: {
            margin: 10,
            textStyle: {
              fontSize: 14
            }
          },
          splitLine: {
            lineStyle: {
              color: 'rgba(194, 239, 179, 0.28)'
            }
          }
        }],
        series: [{
          name: 'Temperatura',
          type: 'line',
          smooth: true,
          symbol: 'circle',
          symbolSize: 5,
          showSymbol: false,
          lineStyle: {
            normal: {
              width: 1
            }
          },
          areaStyle: {
            normal: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
                offset: 0,
                color: 'rgba(0, 136, 212, 0.3)'
              }, {
                offset: 0.8,
                color: 'rgba(0, 136, 212, 0)'
              }], false),
              shadowColor: 'rgba(0, 0, 0, 0.1)',
              shadowBlur: 10
            }
          },
          itemStyle: {
            normal: {
              color: 'rgb(0,136,212)',
              borderColor: 'rgba(0,136,212,0.2)',
              borderWidth: 12

            }
          },
          data: []
        }]
      })

      setInterval(this.fetchData(), 10000);

    }
  }
}
</script>
