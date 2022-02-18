<template>
  <div style="position: relative; display: block; width: 100%;border-top: 1px #000000 dashed;">
    <div style="border-right: 1px #000000 dashed; width: 50%;height: 480px;border-left: 1px #000000 dashed;">

      <!-- //员工性别分布 -->
      <div id="mainy1" style="width: 630px;height:405px; float: left;"></div>
    </div>
    <div style="  width: 50%;height: 440px; position: absolute;top: 0px;right: 2px; ">
      <!-- //员工婚姻分布 -->
      <div id="mainy2" style="width: 630px;height:405px; float: right;"></div>
    </div>
  </div>



  <div style="position: relative; display: block; width: 100%;border-top: 1px #000000 dashed;">
    <div style="border-left: 1px #000000 dashed; width: 50%;height: 480px;border-right: 1px #000000 dashed;">

      <!--  员工司龄分布 -->
      <div id="mainy3" style="width: 630px;height:405px; float: left;"></div>
    </div>
    <div style="  width: 50%;height: 440px; position: absolute;top: 0px;right: 2px; ">
      <!-- 员工学历分布 -->
      <div id="mainy4" style="width: 630px;height:405px; float: right;"></div>
    </div>
  </div>


  <div style="position: relative; display: block; width: 100%;border-top: 1px #000000 dashed;">
    <div style="border-left: 1px #000000 dashed; width: 50%;height: 480px;border-right: 1px #000000 dashed;">

      <!-- 员工年龄段分布 -->
      <div id="mainy5" style="width: 630px;height:405px; float: left;"></div>
    </div>
    <div style="  width: 50%;height: 450px; position: absolute;top: 0px;right: 2px; ">
      <!-- 	员工年代分布 -->
      <div id="mainy6" style="width: 630px;height:405px; float: right;"></div>
    </div>


    <div style="position: relative; display: block; width: 100%;border-top: 1px #000000 dashed;">
      <div style="border-left: 1px #000000 dashed; width: 50%;height: 480px;border-right: 1px #000000 dashed;">

        <!-- 员工年龄段分布 -->
        <div id="mainy7" style="width: 630px;height:405px; float: left;"></div>
      </div>
      <div style="  width: 50%;height: 440px; position: absolute;top: 0px;right: 2px; ">
        <!-- 	员工年代分布 -->
        <div id="mainy8" style="width: 630px;height:405px; float: right;"></div>
      </div>
    </div>
  </div>
</template>

<script>
import {
  defineComponent,
  reactive,
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

    }
  },

  data() {
    return {



    }
  },
  computed: {

  },
  mounted() {
    //准备实例
    /*  员工性别分布 */
    var chartDomy1 = document.getElementById('mainy1');
    var myCharty1 = echarts.init(chartDomy1);
    var optiony1;
    /*   员工婚姻分布 */
    var chartDomy2 = document.getElementById('mainy2');
    var myCharty2 = echarts.init(chartDomy2);
    var optiony2;
    /* 员工司龄分布 */
    var chartDomy3 = document.getElementById('mainy3');
    var myCharty3 = echarts.init(chartDomy3);
    var optiony3;
    /* <!-- 员工学历分布 --> */
    var chartDomy4 = document.getElementById('mainy4');
    var myCharty4 = echarts.init(chartDomy4);
    var optiony4;
    /* <!-- 	员工年代分布 --> */
    var chartDomy6 = document.getElementById('mainy6');
    var myCharty6 = echarts.init(chartDomy6);
    var optiony6;
    /* 	<!-- 员工年龄段分布 --> */
    var chartDomy5 = document.getElementById('mainy5');
    var myCharty5 = echarts.init(chartDomy5);
    var optiony5;

    optiony1 = {
      title: {
        text: '员工性别分布',

        left: 'center'
      },
      tooltip: {
        trigger: 'item'
      },
      legend: {
        orient: 'vertical',
        left: 'left'
      },
      toolbox: {
        feature: {
          dataView: {
            show: true,
            readOnly: false,
            title:"数据视图"
          },
          saveAsImage: {
            show: true,
            title:"保存"
          }
        }
      },
      series: [
        {
          name: 'Access From',
          type: 'pie',
          radius: '50%',
          data: [
            { value: 2, name: '男',itemStyle:{color: '#0941f1'}},

            { value: 3, name: '女',itemStyle:{color: '#ea7c99'} }
          ],
          emphasis: {
            itemStyle: {
              shadowBlur: 10,
              shadowOffsetX: 0,
              shadowColor: 'rgba(0, 0, 0, 0.5)'
            }
          }
        }
      ]
    };
    /* 员工婚姻分布 */
    var colors2 = [
      '#5e81ec',
      '#ffc855',
      '#98e79b',
      '#00d695',
      '#00b29a',
      '#5470c6',
      '#91cc75',
      '#fac858',
      '#ee6666',
      '#73c0de',
      '#3ba272',
      '#fc8452',
      '#9a60b4',
      '#ea7ccc',
    ];
    var data2 = [
      { value: 55, name: '煤' },
      { value: 38, name: '电' },
      { value: 32, name: '油' },
      { value: 30, name: '其他' },
      { value: 28, name: '气' },
    ];
    var total = data2.reduce((p, v) => {
      return p + v.value;
    }, 0);

    optiony2 = {
      title: {
        text: '员工婚姻分布',

        left: 'center'
      },
      tooltip: {
        trigger: 'item'
      },
      legend: {
        orient: 'vertical',
        left: 'left'
      },
      toolbox: {
        feature: {
          dataView: {
            show: true,
            readOnly: false,
            title:"数据视图"
          },
          saveAsImage: {
            show: true,
            title:"保存"
          }
        }
      },
      series: [
        {
          name: 'Access From',
          type: 'pie',
          radius: '50%',
          data: [
            { value: 5, name: '已婚' },
            { value: 4, name: '未婚' },
            { value: 0, name: '离异' },
          ],
          emphasis: {
            itemStyle: {
              shadowBlur: 10,
              shadowOffsetX: 0,
              shadowColor: 'rgba(0, 0, 0, 0.5)'
            }
          }
        }
      ]
    };
    /* 员工司龄分布 */
    var colors3 = [
      '#5e81ec',
      '#ffc855',
      '#98e79b',
      '#00d695',
      '#00b29a',
      '#5470c6',
      '#91cc75',
      '#fac858',
      '#ee6666',
      '#73c0de',
      '#3ba272',
      '#fc8452',
      '#9a60b4',
      '#ea7ccc',
    ];
    var data3 = [
      { value: 55, name: '煤' },
      { value: 38, name: '电' },
      { value: 32, name: '油' },
      { value: 30, name: '其他' },
      { value: 28, name: '气' },
    ];
    var total = data3.reduce((p, v) => {
      return p + v.value;
    }, 0);

    optiony3 = {
      title: {
        text: '员工司龄分布',

        left: 'center'
      },
      tooltip: {
        trigger: 'item'
      },
      legend: {
        orient: 'vertical',
        left: 'left'
      },
      toolbox: {
        feature: {
          dataView: {
            show: true,
            readOnly: false,
            title:"数据视图"
          },
          saveAsImage: {
            show: true,
            title:"保存"
          }
        }
      },
      series: [
        {
          name: 'Access From',
          type: 'pie',
          radius: '50%',
          data: [
            { value: 1048, name: '三年及以下' },
            { value: 735, name: '五年及以下' },
            { value: 580, name: '五年以上' },

          ],
          emphasis: {
            itemStyle: {
              shadowBlur: 10,
              shadowOffsetX: 0,
              shadowColor: 'rgba(0, 0, 0, 0.5)'
            }
          }
        }
      ]
    };
    /* 员工学历分布 */
    var colors4 = [
      '#5e81ec',
      '#ffc855',
      '#98e79b',
      '#00d695',
      '#00b29a',
      '#5470c6',
      '#91cc75',
      '#fac858',
      '#ee6666',
      '#73c0de',
      '#3ba272',
      '#fc8452',
      '#9a60b4',
      '#ea7ccc',
    ];
    var data4 = [
      { value: 55, name: '煤' },
      { value: 38, name: '电' },
      { value: 32, name: '油' },
      { value: 30, name: '其他' },
      { value: 28, name: '气' },
    ];
    var total = data4.reduce((p, v) => {
      return p + v.value;
    }, 0);

    optiony4 = {
      title: {
        text: '员工学历分布',

        left: 'center'
      },
      tooltip: {
        trigger: 'item'
      },
      legend: {
        orient: 'vertical',
        left: 'left'
      },
      toolbox: {
        feature: {
          dataView: {
            show: true,
            readOnly: false,
            title:"数据视图"
          },
          saveAsImage: {
            show: true,
            title:"保存"
          }
        }
      },
      series: [
        {
          name: 'Access From',
          type: 'pie',
          radius: '50%',
          data: [
            { value: 1048, name: '高中' },
            { value: 735, name: '大专' },
            { value: 580, name: '本科' },
            { value: 484, name: '硕士' },
            { value: 300, name: '博士' }
          ],
          emphasis: {
            itemStyle: {
              shadowBlur: 10,
              shadowOffsetX: 0,
              shadowColor: 'rgba(0, 0, 0, 0.5)'
            }
          }
        }
      ]
    };
    /* 员工年龄段分布 */
    var colors5 = [
      '#5e81ec',
      '#ffc855',
      '#98e79b',
      '#00d695',
      '#00b29a',
      '#5470c6',
      '#91cc75',
      '#fac858',
      '#ee6666',
      '#73c0de',
      '#3ba272',
      '#fc8452',
      '#9a60b4',
      '#ea7ccc',
    ];
    var data5 = [
      { value: 55, name: '煤' },
      { value: 38, name: '电' },
      { value: 32, name: '油' },
      { value: 30, name: '其他' },
      { value: 28, name: '气' },
    ];
    var total = data5.reduce((p, v) => {
      return p + v.value;
    }, 0);

    optiony5 = {
      title: {
        text: '员工年龄段分布',

        left: 'center'
      },
      tooltip: {
        trigger: 'item'
      },
      legend: {
        orient: 'vertical',
        left: 'left'
      },
      toolbox: {
        feature: {
          dataView: {
            show: true,
            readOnly: false,
            title:"数据视图"
          },
          saveAsImage: {
            show: true,
            title:"保存"
          }
        }
      },
      series: [
        {
          name: 'Access From',
          type: 'pie',
          radius: '50%',
          data: [
            { value: 1048, name: '二十五岁以下' },
            { value: 735, name: '三十岁以下' },
            { value: 580, name: '三十岁以上' },
          ],
          emphasis: {
            itemStyle: {
              shadowBlur: 10,
              shadowOffsetX: 0,
              shadowColor: 'rgba(0, 0, 0, 0.5)'
            }
          }
        }
      ]
    };

    /*  员工年代分布 */
    var colors6 = [
      '#5e81ec',
      '#ffc855',
      '#98e79b',
      '#00d695',
      '#00b29a',
      '#5470c6',
      '#91cc75',
      '#fac858',
      '#ee6666',
      '#73c0de',
      '#3ba272',
      '#fc8452',
      '#9a60b4',
      '#ea7ccc',
    ];
    var data6 = [
      { value: 55, name: '煤' },
      { value: 38, name: '电' },
      { value: 32, name: '油' },
      { value: 30, name: '其他' },
      { value: 28, name: '气' },
    ];
    var total = data6.reduce((p, v) => {
      return p + v.value;
    }, 0);

    var colors6 = [
      '#5e81ec',
      '#ffc855',
      '#98e79b',
      '#00d695',
      '#00b29a',
      '#5470c6',
      '#91cc75',
      '#fac858',
      '#ee6666',
      '#73c0de',
      '#3ba272',
      '#fc8452',
      '#9a60b4',
      '#ea7ccc',
    ];
    var data6 = [
      { value: 5, name: '水瓶座' },
      { value: 8, name: '双鱼座' },
      { value: 0, name: '摩羯座' },
      { value: 3, name: '射手座' },
      { value: 0, name: '天蝎座' },
      { value: 0, name: '天秤座' },
      { value: 0, name: '处女座' },
      { value: 0, name: '狮子座' },
      { value: 0, name: '巨蟹座' },
      { value: 0, name: '双子座' },
      { value:0, name: '金牛座' },
      { value: 0, name: '白羊座' },
    ];
    var total = data6.reduce((p, v) => {
      return p + v.value;
    }, 0);
    optiony6 = {
      title:{
        text:"员工星座分布",
      },
      toolbox: {
        feature: {
          dataView: {
            show: true,
            readOnly: false,
            title:"数据视图"
          },
          saveAsImage: {
            show: true,
            title:"保存"
          }
        }
      },
      tooltip: {
        trigger: 'item',
        formatter: '{b} : {c} ({d}%)',
      },
      color: colors6,
      calculable: true,
      series: [
        {
          type: 'pie',
          radius: ['30%', '65%'],
          center: ['50%', '50%'],
          roseType: 'radius',
          label: {
            show: true,
            color: 'inherit',
            position: 'outside',
            fontSize: 14,
            formatter:"{b} {d}%",
          },
          labelLine: {
            length: 1,
            length2: 20,
            smooth: true,
          },
          data: data6,
        },
      ],
    };
  /*  在职员工状态分布*/
    // var colors8 = [
    //   '#5e81ec',
    //   '#ffc855',
    //   '#98e79b',
    //   '#00d695',
    //   '#00b29a',
    //   '#5470c6',
    //   '#91cc75',
    //   '#fac858',
    //   '#ee6666',
    //   '#73c0de',
    //   '#3ba272',
    //   '#fc8452',
    //   '#9a60b4',
    //   '#ea7ccc',
    // ];
    // var data8 = [
    //   { value: 55, name: '煤' },
    //   { value: 38, name: '电' },
    //   { value: 32, name: '油' },
    //   { value: 30, name: '其他' },
    //   { value: 28, name: '气' },
    // ];
    // var total = data8.reduce((p, v) => {
    //   return p + v.value;
    // }, 0);
    // optiony8 = {
    //   title:{
    //     text:"在职员工状态分布",
    //   },
    //   toolbox: {
    //     feature: {
    //       dataView: {
    //         show: true,
    //         readOnly: false,
    //         title:"数据视图"
    //       },
    //       saveAsImage: {
    //         show: true,
    //         title:"保存"
    //       }
    //     }
    //   },
    //   tooltip: {
    //     trigger: 'item',
    //     formatter: '{b} : {c} ({d}%)',
    //   },
    //   color: colors8,
    //   calculable: true,
    //   series: [
    //     {
    //       type: 'pie',
    //       radius: ['30%', '65%'],
    //       center: ['50%', '50%'],
    //       roseType: 'radius',
    //       label: {
    //         show: true,
    //         color: 'inherit',
    //         position: 'outside',
    //         fontSize: 14,
    //         formatter:"{b} {d}%",
    //       },
    //       labelLine: {
    //         length: 1,
    //         length2: 20,
    //         smooth: true,
    //       },
    //       data: data8,
    //     },
    //   ],
    // };


    optiony1 && myCharty1.setOption(optiony1);
    optiony2 && myCharty2.setOption(optiony2);
    optiony3 && myCharty3.setOption(optiony3);
    optiony4 && myCharty4.setOption(optiony4);
    optiony5 && myCharty5.setOption(optiony5);
    optiony6 && myCharty6.setOption(optiony6);
    // optiony7 && myCharty7.setOption(optiony7);
    // optiony8 && myCharty8.setOption(optiony8);

  }
}
</script>

<style scoped>

</style>