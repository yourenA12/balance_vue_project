<template>
  <div class="demo-date-picker" style="width: 100%;height: 49px;">
    <div class="" style="width: 850px;height: 49px; float: right;">
      <span class="demonstration" style="position: relative;top: -1px;">时间范围：</span>

      <el-date-picker
          v-model="value2"
          type="monthrange"
          unlink-panels
          range-separator="To"
          start-placeholder="Start month"
          end-placeholder="End month"
          :shortcuts="shortcuts"

      >
      </el-date-picker>
      <span class="demonstration" style="position: relative;top: -1px;left: 120px;">组织范围：</span>
      <el-select v-model="valuee" placeholder="请选择" style="left: 120px;">
        <el-option
            v-for="item in optionss"
            :key="item.valuee"
            :label="item.labell"
            :value="item.valuee"
            style="position: relative;top: -1px;"
        >
        </el-option>
      </el-select>
      <el-button type="primary" style="position: relative;top: -1px;left: 130px;">搜索</el-button>
    </div>

  </div>
  <div style="position: relative; display: block; width: 100%;border-top: 1px #000000 dashed;">
    <!--  每月计薪人数 -->
    <div id="main" style="width: 1050px;height:460px; margin-top: 20px; "></div>
    <div style="width: 16%;height: 320px; float: right; margin-top: -390px;">
      <span class="ziti">当前月计薪人数：</span>
      <br>
      <span class="ziti">3</span>
      <br>
      <span class="ziti">相比上月：</span>
      <br>
      <span class="ziti">???</span>
    </div>
  </div>
  <div style="position: relative; display: block; width: 100%;border-top: 1px #000000 dashed;">
    <!--  每月人力成本总额 -->
    <div id="main1" style="width: 1050px;height:460px; margin-top: 20px; "></div>
    <div style="width: 16%;height: 320px; float: right; margin-top: -390px;">
      <span class="ziti">每月实发工资总额：</span>
      <br>
      <span class="ziti">3</span>
      <br>
      <span class="ziti">相比上月：</span>
      <br>
      <span class="ziti">???</span>
    </div>
  </div>

</template>

<script>
import {
  defineComponent,
  reactive, ref,
  toRefs
} from 'vue'
import * as echarts from 'echarts'
import 'echarts-gl';
export default {
  setup() {
    const state = reactive({
      shortcuts: [{
        text: 'Last week',
        value: () => {
          const end = new Date()
          const start = new Date()
          start.setTime(start.getTime() - 3600 * 1000 * 24 * 7)
          return [start, end]
        },
      },
        {
          text: 'Last month',
          value: () => {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30)
            return [start, end]
          },
        },
        {
          text: 'Last 3 months',
          value: () => {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 90)
            return [start, end]
          },
        },
      ],
      value1: '',
      value2: '',
    })

    return {
      ...toRefs(state),
      optionss: ref([
        {
          valuee: 'Option1',
          labell: 'Option1',
        },
        {
          valuee: 'Option2',
          labell: 'Option2',
        },
        {
          valuee: 'Option3',
          labell: 'Option3',
        },
        {
          valuee: 'Option4',
          labell: 'Option4',
        },
        {
          valuee: 'Option5',
          labell: 'Option5',
        },
      ]),
      valuee: ref(''),


    }
  },

  data() {
    return {

    }
  },
  mounted() {
    //准备实例
    /* 	<!--  每月计薪人数--> */
    var chartDom = document.getElementById('main');
    var myChart = echarts.init(chartDom);
    /* 	每月人力成本总额 */
    var chartDom1 = document.getElementById('main1');
    var myChart1 = echarts.init(chartDom1);

    var option;
    var option1;

    /* <!-- 每月计薪人数 --> */

    // option = {
    //   title: {
    //     text: ''
    //
    //   },
    //   tooltip: {
    //     trigger: 'axis',
    //     axisPointer: {
    //       type: 'cross',
    //       crossStyle: {
    //         color: '#999'
    //       }
    //     }
    //   },
    //   toolbox: {
    //     feature: {
    //       dataView: {
    //         show: true,
    //         readOnly: false,
    //         title:"数据视图"
    //       },
    //       magicType: {
    //         show: true,
    //         type: ['line', 'bar'],
    //         title:{
    //           line:"折线图",
    //           bar:"柱状图",
    //         }
    //       },
    //       restore: {
    //         show: true,
    //         title:"还原"
    //       },
    //       saveAsImage: {
    //         show: true,
    //         title:"保存"
    //       }
    //     }
    //   },
    //   legend: {
    //     data: ['计薪人数', 'Precipitation', '增长率']
    //   },
    //   xAxis: [{
    //     type: 'category',
    //     data: ['2021-11-29', '2021-11-30', '2021-11-29', '2021-11-30', '2021-12-01', '2021-12-02',
    //       '2021-12-03'
    //     ],
    //     axisPointer: {
    //       type: 'shadow'
    //     }
    //   }],
    //   yAxis: [{
    //     type: 'value',
    //     name: '人数(人)',
    //     min: 0,
    //     max: 250,
    //     interval: 50,
    //     axisLabel: {
    //       formatter: '{value} 人'
    //     }
    //   },
    //     {
    //       type: 'value',
    //       name: '增长律（%）',
    //       min: 0,
    //       max: 100,
    //       interval: 10,
    //       axisLabel: {
    //         formatter: '{value} %'
    //       }
    //     },
    //
    //   ],
    //   series: [{
    //     name: '计薪人数',
    //     type: 'bar',
    //     data: [
    //       135.6, 162.2, 32.6, 20.0, 6.4, 3.3, 44
    //     ],barWidth:'25%',
    //   },
    //
    //     {
    //       name: '增长率',
    //       type: 'line',
    //       yAxisIndex: 1,
    //       data: [10, 20, 30, 40, 50, 30, 40],
    //       itemStyle: {
    //         normal: {
    //           color: "#FFFF66", //折线点的颜色
    //           lineStyle: {
    //             color: "#FFFF66" //折线的颜色
    //           }
    //         }
    //       },
    //     }
    //   ]
    // };
    option = {
      title: {
        text: '每月计薪人数',
        subtext: 'Fake Data'
      },
      tooltip: {
        trigger: 'axis'
      },
      legend: {
        data: ['Rainfall', 'Evaporation']
      },
      toolbox: {
        show: true,
        feature: {
          dataView: { show: true, readOnly: false },
          magicType: { show: true, type: ['line', 'bar'] },
          restore: { show: true },
          saveAsImage: { show: true }
        }
      },
      calculable: true,
      xAxis: [
        {
          type: 'category',
          // prettier-ignore
          data: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
        }
      ],
      yAxis: [
        {
          type: 'value'
        }
      ],
      series: [
        {
          name: 'Rainfall',
          type: 'bar',
          data: [
            2.0, 4.9, 7.0, 23.2, 25.6, 76.7, 135.6, 162.2, 32.6, 20.0, 6.4, 3.3
          ],
          markPoint: {
            data: [
              { type: 'max', name: 'Max' },
              { type: 'min', name: 'Min' }
            ]
          },
          markLine: {
            data: [{ type: 'average', name: 'Avg' }]
          }
        },
        {
          name: 'Evaporation',
          type: 'bar',
          data: [
            2.6, 5.9, 9.0, 26.4, 28.7, 70.7, 175.6, 182.2, 48.7, 18.8, 6.0, 2.3
          ],
          markPoint: {
            data: [
              { name: 'Max', value: 182.2, xAxis: 7, yAxis: 183 },
              { name: 'Min', value: 2.3, xAxis: 11, yAxis: 3 }
            ]
          },
          markLine: {
            data: [{ type: 'average', name: 'Avg' }]
          }
        }
      ]
    };
    /* 每月人力成本总额 */
    // option1 = {
    //   title: {
    //     text: '每月人力成本总额'
    //   },
    //   tooltip: {
    //     trigger: 'axis',
    //     axisPointer: {
    //       type: 'cross',
    //       crossStyle: {
    //         color: '#999'
    //       }
    //     }
    //   },
    //   toolbox: {
    //     feature: {
    //       dataView: {
    //         show: true,
    //         readOnly: false,
    //         title:"数据视图"
    //       },
    //       magicType: {
    //         show: true,
    //         type: ['line', 'bar'],
    //         title:{
    //           line:"折线图",
    //           bar:"柱状图",
    //         }
    //       },
    //       restore: {
    //         show: true,
    //         title:"还原"
    //       },
    //       saveAsImage: {
    //         show: true,
    //         title:"保存"
    //       }
    //     }
    //   },
    //   legend: {
    //     data: ['人力成本', '增长率']
    //   },
    //   xAxis: [{
    //     type: 'category',
    //     data: ['2021-11-29', '2021-11-30', '2021-11-29', '2021-11-30', '2021-12-01', '2021-12-02',
    //       '2021-12-03'
    //     ],
    //     axisPointer: {
    //       type: 'shadow'
    //     }
    //   }],
    //   yAxis: [{
    //     type: 'value',
    //     name: '元',
    //     min: 0,
    //     max: 250,
    //     interval: 50,
    //     axisLabel: {
    //       formatter: '{value} 元'
    //     }
    //   },
    //     {
    //       type: 'value',
    //       name: '增长律（%）',
    //       min: 0,
    //       max: 100,
    //       interval: 10,
    //       axisLabel: {
    //         formatter: '{value} %'
    //       }
    //     },
    //
    //   ],
    //   series: [{
    //     name: '人力成本',
    //     type: 'bar',
    //     data: [
    //       7.0, 23.2, 25.6, 135.6, 162.2, 32.6, 20.0, 6.4,
    //     ],barWidth:'25%',
    //   },
    //
    //     {
    //       name: '增长率',
    //       type: 'line',
    //       yAxisIndex: 1,
    //       data: [10, 20, 30, 40, 50, 30, 40],
    //       itemStyle: {
    //         normal: {
    //           color: "#FFFF66", //折线点的颜色
    //           lineStyle: {
    //             color: "#FFFF66" //折线的颜色
    //           }
    //         }
    //       },
    //     }
    //   ]
    // };
    //每月实发工资总额
    option1 = {
      title: {
        text: '每月实发工资总额'
      },
      tooltip: {
        trigger: 'axis',
        axisPointer: {
          type: 'cross',
          crossStyle: {
            color: '#999'
          }
        }
      },
      toolbox: {
        feature: {
          dataView: {
            show: true,
            readOnly: false,
            title:"数据视图"
          },
          magicType: {
            show: true,
            type: ['line', 'bar'],
            title:{
              line:"折线图",
              bar:"柱状图",
            }
          },
          restore: {
            show: true,
            title:"还原"
          },
          saveAsImage: {
            show: true,
            title:"保存"
          }
        }
      },
      legend: {
        data: ['实发工资', '增长率']
      },
      xAxis: [{
        type: 'category',
        data: ['2021-11-29', '2021-11-30', '2021-11-29', '2021-11-30', '2021-12-01', '2021-12-02',
          '2021-12-03'
        ],
        axisPointer: {
          type: 'shadow'
        }
      }],
      yAxis: [{
        type: 'value',
        name: '元',
        min: 0,
        max: 250,
        interval: 50,
        axisLabel: {
          formatter: '{value} 元'
        }
      },
        {
          type: 'value',
          name: '增长律（%）',
          min: 0,
          max: 100,
          interval: 10,
          axisLabel: {
            formatter: '{value} %'
          }
        },

      ],
      series: [{
        name: '实发工资',
        type: 'bar',
        data: [
          44, 23, 56, 77, 88, 44, 33, 66
        ],barWidth:'25%',
      },

        {
          name: '增长率',
          type: 'line',
          yAxisIndex: 1,
          data: [10, 20, 30, 40, 50, 30, 40],
          itemStyle: {
            normal: {
              color: "#FFFF66", //折线点的颜色
              lineStyle: {
                color: "#FFFF66" //折线的颜色
              }
            }
          },
        }
      ]
    };
    /* 每月平均工资 */




    option && myChart.setOption(option);
    option1 && myChart1.setOption(option1);


  }
}
</script>


<style scoped>
@import url("../../css/Statistics.css");

 .el-menu[data-v-244d4530] {
   height: 57px;
   font-weight: bold;
 }
</style>