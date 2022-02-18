<!--人员数量分袖-->
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
    <!--  每月参保人数 -->
    <div id="mainy1" style="width: 1030px;height:460px; margin-top: 20px; "></div>
    <div style="width: 16%;height: 320px; float: right; margin-top: -390px;">
      <span class="ziti">当前月参保人数：</span>
      <br>
      <span class="ziti">3</span>
      <br>
      <span class="ziti">相比上月：</span>
      <br>
      <span class="ziti">3</span>
    </div>
  </div>
  <div style="position: relative; display: block; width: 100%;border-top: 1px #000000 dashed;">
    <!--  每月社保缴纳总额 -->
    <div id="mainy2" style="width: 1030px;height:460px; margin-top: 20px; "></div>
    <div style="width: 16%;height: 320px; float: right; margin-top: -390px;">
      <span class="ziti">当前月公司社保总额：</span>
      <br>
      <span class="ziti">3</span>
      <br>
      <span class="ziti">当前月个人社保总额：</span>
      <br>
      <span class="ziti">3</span><br>
      <span class="ziti">相比上月：</span><br>
      <span class="ziti">3</span>
    </div>
  </div>
  <div style="position: relative; display: block; width: 100%;border-top: 1px #000000 dashed;">
    <!--  每月公积金缴纳总额-->
    <div id="mainy3" style="width: 1030px;height:460px; margin-top: 20px; "></div>
    <div style="width: 16%;height: 320px; float: right; margin-top: -390px;">
      <span class="ziti">当前月公司公积金总额：</span>
      <br>
      <span class="ziti">3</span>
      <br>
      <span class="ziti">当前月个人公积金总额：</span>
      <br>
      <span class="ziti">123</span><br>
      <span class="ziti">相比上月：</span><br>
      <span class="ziti">1223</span>
    </div>
  </div>
  <div style="position: relative; display: block; width: 100%;border-top: 1px #000000 dashed;">
    <!--  每月平均工资 -->
    <div id="mainy4" style="width: 1030px;height:460px; margin-top: 20px; "></div>
    <div style="width: 16%;height: 320px; float: right; margin-top: -390px;">
      <span class="ziti">当前月人均工资：</span>
      <br>
      <span class="ziti">3</span>
      <br>
      <span class="ziti">相比上月：</span>
      <br>
      <span class="ziti">3</span>
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
      tableData: [{
        date: '2016-05-03',
        name: 'Tom',
        state: 'California',
        city: 'Los Angeles',
        address: 'No. 189, Grove St, Los Angeles',
        zip: 'CA 90036',
      },
        {
          date: '2016-05-02',
          name: 'Tom',
          state: 'California',
          city: 'Los Angeles',
          address: 'No. 189, Grove St, Los Angeles',
          zip: 'CA 90036',
        },
        {
          date: '2016-05-04',
          name: 'Tom',
          state: 'California',
          city: 'Los Angeles',
          address: 'No. 189, Grove St, Los Angeles',
          zip: 'CA 90036',
        },
        {
          date: '2016-05-01',
          name: 'Tom',
          state: 'California',
          city: 'Los Angeles',
          address: 'No. 189, Grove St, Los Angeles',
          zip: 'CA 90036',
        },
        {
          date: '2016-05-08',
          name: 'Tom',
          state: 'California',
          city: 'Los Angeles',
          address: 'No. 189, Grove St, Los Angeles',
          zip: 'CA 90036',
        },
        {
          date: '2016-05-06',
          name: 'Tom',
          state: 'California',
          city: 'Los Angeles',
          address: 'No. 189, Grove St, Los Angeles',
          zip: 'CA 90036',
        },
        {
          date: '2016-05-07',
          name: 'Tom',
          state: 'California',
          city: 'Los Angeles',
          address: 'No. 189, Grove St, Los Angeles',
          zip: 'CA 90036',
        },
      ],
    }
  },
  mounted() {
    //准备实例
    /* 	<!--  每月参保人数--> */
    var chartDomy1 = document.getElementById('mainy1');
    var myCharty1 = echarts.init(chartDomy1);
    /* 	每月缴纳保险总额 */
    var chartDomy2 = document.getElementById('mainy2');
    var myCharty2 = echarts.init(chartDomy2);
    /* 每月社保缴纳总额 */
    var chartDomy3 = document.getElementById('mainy3');
    var myCharty3 = echarts.init(chartDomy3);
    /* 每月公积金缴纳总额*/
    var chartDomy4 = document.getElementById('mainy4');
    var myCharty4 = echarts.init(chartDomy4);

    var optiony4;
    var optiony1;
    var optiony2;
    var optiony3;

    optiony1 = {
      title: {
        text: '每月参保人数',
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
    /* 每月缴纳保险总额 */
    // optiony2 = {
    //   title: {
    //     text: ''
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
    //     data: ['保险总额', '增长率']
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
    //     name: '保险总额',
    //     type: 'bar',
    //     data: [
    //       200, 149, 27.0, 23.2, 225.6, 276.7, 135.6
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
    optiony2 = {
      title: {
        text: '每月缴纳保险总额'
      },
      tooltip: {
        trigger: 'axis',
        axisPointer: {
          type: 'cross',
          label: {
            backgroundColor: '#6a7985'
          }
        }
      },
      legend: {
        data: ['Email', 'Union Ads', 'Video Ads', 'Direct', 'Search Engine']
      },
      toolbox: {
        feature: {
          saveAsImage: {}
        }
      },
      grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true
      },
      xAxis: [
        {
          type: 'category',
          boundaryGap: false,
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
        }
      ],
      yAxis: [
        {
          type: 'value'
        }
      ],
      series: [
        {
          name: 'Email',
          type: 'line',
          stack: 'Total',
          areaStyle: {},
          emphasis: {
            focus: 'series'
          },
          data: [120, 132, 101, 134, 90, 230, 210]
        },
        {
          name: 'Union Ads',
          type: 'line',
          stack: 'Total',
          areaStyle: {},
          emphasis: {
            focus: 'series'
          },
          data: [220, 182, 191, 234, 290, 330, 310]
        },
        {
          name: 'Video Ads',
          type: 'line',
          stack: 'Total',
          areaStyle: {},
          emphasis: {
            focus: 'series'
          },
          data: [150, 232, 201, 154, 190, 330, 410]
        },
        {
          name: 'Direct',
          type: 'line',
          stack: 'Total',
          areaStyle: {},
          emphasis: {
            focus: 'series'
          },
          data: [320, 332, 301, 334, 390, 330, 320]
        },
        {
          name: 'Search Engine',
          type: 'line',
          stack: 'Total',
          label: {
            show: true,
            position: 'top'
          },
          areaStyle: {},
          emphasis: {
            focus: 'series'
          },
          data: [820, 932, 901, 934, 1290, 1330, 1320]
        }
      ]
    };
    /* 每月社保缴纳总额 */
    // optiony3 = {
    //   title: {
    //     text: ''
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
    //     data: ['公司社保总额', '个人社保总额', '增长率']
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
    //     name: '公司社保总额',
    //     type: 'bar',
    //     data: [
    //       2.0, 4.9, 7.0, 23.2, 25.6, 76.7, 135.6, 162.2, 32.6, 20.0, 6.4, 3.3
    //     ]
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
    //     },
    //     {
    //       name: '个人社保总额',
    //       type: 'bar',
    //       data: [
    //         2.0, 4.9, 7.0, 23.2, 25.6, 76.7, 135.6, 162.2, 32.6, 20.0, 6.4, 3.3
    //       ]
    //     },
    //   ]
    // };
    optiony3 = {
      title: {
        text: '每月社保缴纳总额'
      },
      tooltip: {
        trigger: 'axis',
        axisPointer: {
          type: 'cross',
          label: {
            backgroundColor: '#6a7985'
          }
        }
      },
      legend: {
        data: ['Email', 'Union Ads', 'Video Ads', 'Direct', 'Search Engine']
      },
      toolbox: {
        feature: {
          saveAsImage: {}
        }
      },
      grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true
      },
      xAxis: [
        {
          type: 'category',
          boundaryGap: false,
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
        }
      ],
      yAxis: [
        {
          type: 'value'
        }
      ],
      series: [
        {
          name: 'Email',
          type: 'line',
          stack: 'Total',
          areaStyle: {},
          emphasis: {
            focus: 'series'
          },
          data: [120, 132, 101, 134, 90, 230, 210]
        },
        {
          name: 'Union Ads',
          type: 'line',
          stack: 'Total',
          areaStyle: {},
          emphasis: {
            focus: 'series'
          },
          data: [220, 182, 191, 234, 290, 330, 310]
        },
        {
          name: 'Video Ads',
          type: 'line',
          stack: 'Total',
          areaStyle: {},
          emphasis: {
            focus: 'series'
          },
          data: [150, 232, 201, 154, 190, 330, 410]
        },
        {
          name: 'Direct',
          type: 'line',
          stack: 'Total',
          areaStyle: {},
          emphasis: {
            focus: 'series'
          },
          data: [320, 332, 301, 334, 390, 330, 320]
        },
        {
          name: 'Search Engine',
          type: 'line',
          stack: 'Total',
          label: {
            show: true,
            position: 'top'
          },
          areaStyle: {},
          emphasis: {
            focus: 'series'
          },
          data: [820, 932, 901, 934, 1290, 1330, 1320]
        }
      ]
    };
    /* 每月公积金缴纳总额 */
    optiony4 = {
      title: {
        text: '每月公积金缴纳总额'
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
        data: ['企业公积金总额', '个人公积金总额', '增长率']
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
        max: 300,
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
        name: '企业公积金总额',
        type: 'bar',
        data: [
          2.0, 4.9, 7.0, 23.2, 25.6, 76.7, 135.6, 162.2, 32.6, 20.0, 6.4, 3.3
        ]
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
        },
        {
          name: '个人公积金总额',
          type: 'bar',
          data: [
            200, 49, 70, 232, 256, 76.7, 135.6
          ]
        },
      ]
    };




    optiony1 && myCharty1.setOption(optiony1);
    optiony2 && myCharty2.setOption(optiony2);
    optiony3 && myCharty3.setOption(optiony3);
    optiony4 && myCharty4.setOption(optiony4);

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