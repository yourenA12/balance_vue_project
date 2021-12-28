<template>
  <!-- 页面背景以及页面主身体 -->

  <div class="body_backgrund" >
    <div class="body_2" >
      <div>
        <!-- 欢迎卡片 -->
        <div>
          <div class="hp_crld_left" style="margin-top: 100px">
            <!-- 放头像的div -->
            <div class="photo_1" style="display: inline-block;">

            </div>
            <!-- 欢迎语句 -->
            <div class="text_left_crld" style="display: inline-block;">
              <p>欢迎你：XXX，祝你工作顺利<br>
                <span class="font_xu">公司的 | 超级大 | boss</span></p>
            </div>
          </div>
          <!-- 待办事项div -->
          <div class="hp_crld_ringht" style="margin-top: 100px">

            <div class="hp_crld_ringht_dpsx">
							<span>
								<el-icon class="i_con_svg">
									<i-tickets />
								</el-icon>
								待办事项
							</span>
              <br>
              <br>
              <!-- 入职代办 -->
              <div class="hp_crld_ringht_dpsx_sj">
                <div class="font_wz_size">
                  <div class="font_wz_size_1">
                    <span style="font-weight: bold;font-size: 18px;">0</span><br>
                    <span style="font-size: 14px;color: #2f2b2b;">入职代办</span>
                  </div>
                </div>
              </div>
              <!-- 离职代办 -->
              <div class="hp_crld_ringht_dpsx_sj">
                <div class="font_wz_size">
                  <div class="font_wz_size_1">
                    <span style="font-weight: bold;font-size: 18px;">0</span><br>
                    <span style="font-size: 14px;color: #2f2b2b;">入职代办</span>
                  </div>
                </div>
              </div>
              <!-- 员工审批 -->
              <div class="hp_crld_ringht_dpsx_sj">
                <div class="font_wz_size">
                  <div class="font_wz_size_1">
                    <span style="font-weight: bold;font-size: 18px;">0</span><br>
                    <span style="font-size: 14px;color: #2f2b2b;">入职代办</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- 便捷入口div -->
        <div class="body_center_bjrk">
          <el-col :span="8">
            <el-card shadow="hover">
              <el-icon>
                <i-avatar class="i_icon_svg" />
              </el-icon>
              <span class="right_font">用户</span>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover">
              <el-icon>
                <i-avatar class="i_icon_svg" />
              </el-icon>
              <span class="right_font">用户</span>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover">
              <el-icon>
                <i-avatar class="i_icon_svg" />
              </el-icon>
              <span class="right_font">用户</span>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover">
              <el-icon>
                <i-avatar class="i_icon_svg" />
              </el-icon>
              <span class="right_font">用户</span>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover">
              <el-icon>
                <i-avatar class="i_icon_svg" />
              </el-icon>
              <span class="right_font">用户</span>
            </el-card>
          </el-col>

          <div class="right_float_gg">
            <el-tabs type="card" @tab-click="handleClick">
              <el-tab-pane label="公司公告"></el-tab-pane>
              <el-tab-pane label="Config">Config</el-tab-pane>
              <el-tab-pane label="Role">Role</el-tab-pane>
              <el-tab-pane label="Task">Task</el-tab-pane>
            </el-tabs>

          </div>




        </div>


      </div>
    </div>
    <div class="left_data">
      <el-calendar size="mini" ref="calendar">
        <template #header="{ date }">
          <span>日历日程</span>
          <span>{{ date }}</span>
          <el-button-group>
            <el-button size="mini" @click="selectDate('prev-year')"
            >上一年</el-button
            >
            <el-button size="mini" @click="selectDate('prev-month')"
            >上一月</el-button
            >
            <el-button size="mini" @click="selectDate('today')">今天</el-button>
            <el-button size="mini" @click="selectDate('next-month')"
            >下一月</el-button
            >
            <el-button size="mini" @click="selectDate('next-year')"
            >下一年</el-button
            >
          </el-button-group>
        </template>
      </el-calendar>
    </div>
    <div class="left_data_right">
      <div class="top_data_right">
        今日提醒
      </div>
    </div>
    <br>
    <div id="main1" class="tong_left">

    </div>
    <div id="main2" class="tong_left">

    </div>
  </div>

</template>

<script>

</script>

<style scoped>
/*导入主界面外部样式*/
@import url("../../css/work_min.css");
</style>

<script>
import * as echarts from 'echarts';
import { defineComponent, ref } from 'vue'

export default {
  setup() {
    const calendar = ref()
    const selectDate = (val) => {
      calendar.value.selectDate(val)
    }

    return {
      calendar,
      selectDate,
    }
  },
  data() {
    return {
      activeName: 'first',
    }
  },
  methods: {
    handleClick(tab, event) {
      console.log(tab, event)
    },
  },
  mounted() {
    var chartDom = document.getElementById('main1');
    var myChart = echarts.init(chartDom);
    var option;

    var chartDom1 = document.getElementById('main2');
    var myChart1 = echarts.init(chartDom1);
    var option1;

    option1 = {
      title: {
        text: ''
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
          data: ['星期一', '星期二', '星期三', '星期四', '星期五', '星期六', '星期日']
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
    option = {
      color: ['#80FFA5', '#00DDFF', '#37A2FF', '#FF0087', '#FFBF00'],
      title: {
        text: ''
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
        data: ['Line 1', 'Line 2', 'Line 3', 'Line 4', 'Line 5']
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
          name: 'Line 1',
          type: 'line',
          stack: 'Total',
          smooth: true,
          lineStyle: {
            width: 0
          },
          showSymbol: false,
          areaStyle: {
            opacity: 0.8,
            color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
              {
                offset: 0,
                color: 'rgb(128, 255, 165)'
              },
              {
                offset: 1,
                color: 'rgb(1, 191, 236)'
              }
            ])
          },
          emphasis: {
            focus: 'series'
          },
          data: [140, 232, 101, 264, 90, 340, 250]
        },
        {
          name: 'Line 2',
          type: 'line',
          stack: 'Total',
          smooth: true,
          lineStyle: {
            width: 0
          },
          showSymbol: false,
          areaStyle: {
            opacity: 0.8,
            color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
              {
                offset: 0,
                color: 'rgb(0, 221, 255)'
              },
              {
                offset: 1,
                color: 'rgb(77, 119, 255)'
              }
            ])
          },
          emphasis: {
            focus: 'series'
          },
          data: [120, 282, 111, 234, 220, 340, 310]
        },
        {
          name: 'Line 3',
          type: 'line',
          stack: 'Total',
          smooth: true,
          lineStyle: {
            width: 0
          },
          showSymbol: false,
          areaStyle: {
            opacity: 0.8,
            color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
              {
                offset: 0,
                color: 'rgb(55, 162, 255)'
              },
              {
                offset: 1,
                color: 'rgb(116, 21, 219)'
              }
            ])
          },
          emphasis: {
            focus: 'series'
          },
          data: [320, 132, 201, 334, 190, 130, 220]
        },
        {
          name: 'Line 4',
          type: 'line',
          stack: 'Total',
          smooth: true,
          lineStyle: {
            width: 0
          },
          showSymbol: false,
          areaStyle: {
            opacity: 0.8,
            color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
              {
                offset: 0,
                color: 'rgb(255, 0, 135)'
              },
              {
                offset: 1,
                color: 'rgb(135, 0, 157)'
              }
            ])
          },
          emphasis: {
            focus: 'series'
          },
          data: [220, 402, 231, 134, 190, 230, 120]
        },
        {
          name: 'Line 5',
          type: 'line',
          stack: 'Total',
          smooth: true,
          lineStyle: {
            width: 0
          },
          showSymbol: false,
          label: {
            show: true,
            position: 'top'
          },
          areaStyle: {
            opacity: 0.8,
            color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
              {
                offset: 0,
                color: 'rgb(255, 191, 0)'
              },
              {
                offset: 1,
                color: 'rgb(224, 62, 76)'
              }
            ])
          },
          emphasis: {
            focus: 'series'
          },
          data: [220, 302, 181, 234, 210, 290, 150]
        }
      ]
    };
    option && myChart.setOption(option);
    option1 && myChart1.setOption(option1);
  }
}



</script>