<template>
  <!-- 页面背景以及页面主身体 -->

  <div class="body_backgrund">
    <div class="body_2">
      <div>
        <!-- 欢迎卡片 -->
        <div>
          <div class="hp_crld_left" style="margin-top: 100px">
            <!-- 放头像的div -->
            <div class="photo_1" style="display: inline-block;">

            </div>
            <!-- 欢迎语句 -->
            <div class="text_left_crld" style="display: inline-block;width: 570px">
              <h3>欢迎你：XXX，祝你工作顺利<br>
                <el-popover
                    ref="popover"
                    placement="right"
                    :width="400"
                    trigger="click"
                >

                  <div>
                    <span>  <i class="iconfont">&#xe67f;</i> <b
                        style="position: absolute;top: 29px;">{{ getdates() }}</b></span>
                    <div>
                      <el-timeline>
                        <el-timeline-item timestamp="上班时间" placement="top">
                          <el-button @click="cardadd()" v-show="this.staffClock.mornClock==null"
                                     style="float: right;width: 80px;margin-right: 43px;margin-top: -25px;"
                                     type="primary">上班打卡
                          </el-button>
                          <div>
                            <span style="font-size: 12px;padding-right: 10px;">打卡时间 : {{ staffClock.mornClock }}</span>
                            <p>&nbsp;</p>

                          </div>
                        </el-timeline-item>

                        <el-timeline-item timestamp="下班时间" placement="top">
                          <el-button @click="amend()" v-show="this.staffClock.afternoonClock==null"
                                     style="float: right;width: 80px;margin-right: 43px;margin-top: -25px;"
                                     type="primary">下班打卡
                          </el-button>
                          <div>
                            <span style="font-size: 12px;padding-right: 10px;">打卡时间 : {{
                                staffClock.afternoonClock
                              }}</span>
                            <span style="font-size: 12px"></span>
                          </div>
                        </el-timeline-item>

                      </el-timeline>
                    </div>

                  </div>

                  <template #reference>
                    <el-button style="width: 100px; float:right" type="primary" plain>打卡</el-button>
                  </template>
                </el-popover>
                <span class="font_xu">公司的 | 超级大 | boss</span></h3>

            </div>
          </div>
          <!-- 待办事项div -->
          <div class="hp_crld_ringht" style="margin-top: 100px">

            <div class="hp_crld_ringht_dpsx">
							<span>
								<el-icon class="i_con_svg" style="display: inline-block">
									<i-tickets/>
								</el-icon>
                <h4 style="display: inline-block">待办事项</h4>
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
        <el-icon class="i_con_svg" style="display: inline-block;margin-left: 20px;">
          <i-tickets/>

        </el-icon>
        <h3 style="display: inline-block">便捷入口</h3>
        <div class="body_center_bjrk">

          <el-col :span="8">
            <el-card shadow="hover" class="body_col_1">
              <el-icon>
                <i-avatar class="i_icon_svg"/>
              </el-icon>
              <span class="right_font">审批管理</span>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover" class="body_col_2">
              <el-icon>
                <i-avatar class="i_icon_svg"/>
              </el-icon>
              <span class="right_font">用户</span>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover" class="body_col_3">
              <el-icon>
                <i-avatar class="i_icon_svg"/>
              </el-icon>
              <span class="right_font">用户</span>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover" class="body_col_4">
              <el-icon>
                <i-avatar class="i_icon_svg"/>
              </el-icon>
              <span class="right_font">用户</span>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover" class="body_col_5">
              <el-icon>
                <i-avatar class="i_icon_svg"/>
              </el-icon>
              <span class="right_font">用户</span>
            </el-card>
          </el-col>


          <div class="right_float_gg">
            <div class="noticeV">
              <h3>公司公告:</h3>

            </div>
            <div style="border-bottom: 1px solid #dddddd;"></div>


            <div v-for="(item,index) in tableData1" class="noticeV">

              {{ item.noticeTitle }}

              <el-button class="button_left_xq" type="text" @click="drawer = true">详情</el-button>

            </div>


            <div style="border-bottom: 1px solid #dddddd;"></div>


            <el-drawer v-model="drawer" :with-header="false" title="I am the title" v-for="item in tableData1">
              <div>
                <h1>公告内容：</h1>
              </div>
              <br>
              <div style="margin: 30px">
                {{ item.noticeMatter }}
              </div>
              <el-form :model="item" label-width="">
                <el-form-item label="公告发起人 :">
                  <el-input v-model="item.noticePeople" disabled></el-input>
                </el-form-item>
                <br>
                <el-form-item label="公告类型 :">
                  <el-input v-model="item.noticeTitle" disabled></el-input>
                </el-form-item>
              </el-form>
              <br>
              <br>
              <el-button type="primary" @click="">&nbsp;&nbsp; 确定收到&nbsp;&nbsp;</el-button>
              <el-button style="display: none" type="primary" @click="" disabled>&nbsp;&nbsp; 已确认 &nbsp;&nbsp;
              </el-button>
            </el-drawer>

          </div>


        </div>


      </div>
    </div>
    <div class="left_data">
      <el-calendar size="mini" ref="calendar">
        <template #header="{ date }">
          <h3>日历日程</h3>
          <span>{{ date }}</span>
          <el-button-group>
            <el-button size="mini" @click="selectDate('prev-year')"
            >上一年
            </el-button
            >
            <el-button size="mini" @click="selectDate('prev-month')"
            >上一月
            </el-button
            >
            <el-button size="mini" @click="selectDate('today')">今天</el-button>
            <el-button size="mini" @click="selectDate('next-month')"
            >下一月
            </el-button
            >
            <el-button size="mini" @click="selectDate('next-year')"
            >下一年
            </el-button
            >
          </el-button-group>
        </template>
      </el-calendar>
    </div>
    <div class="left_data_right">
      <div class="top_data_right">
        <h3>今日提醒</h3>
      </div>
    </div>
    <br>


    <div class="tong_left" id="main1">

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

.body_col_1 {
  background: rgb(35, 102, 167);
  color: #fff;
}

.body_col_2 {
  background: #E6A23C;
  color: #fff;
}

.body_col_3 {
  background: #409EFF;
  color: #fff;
}

.body_col_4 {
  background: #909399;
  color: #fff;
}

.body_col_5 {
  background: #F56C6C;
  color: #fff;
}

/*日历样式*/
.el-calendar-table {
  background: #ddd;
}

/* 公司公告 */
.noticeV {
  padding: 10px;
  margin: 10px;
  font-size: 14px;
  /*这里要显示的设置宽度*/
  overflow: hidden;
  white-space: nowrap;
  /*文字超出宽度则显示ellipsis省略号*/
  text-overflow: ellipsis;
  /* background: #0c9c6e;*/
}

.button_left_xq {
  float: right;
}
</style>

<script>
import * as echarts from 'echarts';
import {defineComponent, ref} from 'vue'
import {ElMessage} from "element-plus";

export default {
  setup() {
    const calendar = ref()
    const selectDate = (val) => {
      calendar.value.selectDate(val)
    }

    return {
      calendar,
      selectDate,
      // staffClock:{},


    }
  },
  data() {
    return {
      activeName: 'first',
      tableData1: [],
      drawer: false,

      id: 1,

      staffClock: {
        clockRecordId:'',
        mornClock: null,
        afternoonClock: null
      },

      clockVal: {
        mornClock: new Date(),
        // afternoonClock: new Date(),
        deptId: 1,
        staffId: 1,
        clockRecordId:11,
      }

    }
  },
  //根据员工id查询员工公告
  methods: {
    selectById() {
      //根据id查询
      this.axios
          .get("http://localhost:8010/provider/findNoticeVo/1/3/1")
          .then((response) => {
            console.log(response);
            this.tableData1 = response.data.data.records;
          })
          .catch(function (error) {
            console.log(error);
          })
    },
    handleClick(tab, event) {
      console.log(tab, event)
    },
    //获取当前年月日
    getdates() {
      var date = new Date();
      var seperator1 = "-";
      var year = date.getFullYear();
      var month = date.getMonth() + 1;
      var strDate = date.getDate();

      if (month >= 1 && month <= 9) {
        month = "0" + month;
      }
      if (strDate >= 0 && strDate <= 9) {
        strDate = "0" + strDate;
      }
      var currentdate = year + " 年 " + month + " 月 " + strDate + "日";
      return currentdate;
    },
    //查询员工打卡
    card() {
      this.axios
          .get("http://localhost:8010/provider/card/" + 1)
          .then((response) => {
            if (response.data.data == null) {
              return
            }
            this.staffClock = response.data.data;
          })
          .catch(function (error) {
            console.log(error);
          })
    },
    //添加打卡
    cardadd() {
      this.axios
          .post("http://localhost:8010/provider/add", this.clockVal)
          .then((response) => {

            if (response.data.data === "成功") {
              ElMessage.success("打卡成功")
              this.card()
            } else {
              ElMessage.error("打卡失败")
            }
          }).catch(function (error) {
        console.log(error);
      })
    },
    //修改打卡
    // amend(){
    //
    //   this.axios
    //       .put("http://localhost:8010/provider/amend")
    //       .then((response) => {
    //         data:{
    //           clockRecordId:this.staffClock.clockRecordId;
    //           afternoonClock:this.staffClock.afternoonClock;
    //
    //         }
    //         if( response.data.data ==="成功" ){
    //           ElMessage.success('打卡成功')
    //         }else{
    //           ElMessage.error('打卡失败')
    //         }
    //       }).catch(function (error){
    //     console.log(error);
    //   })
    // }
    amend() {
      this.axios({
        url: 'http://localhost:8010/provider/amend',
        method: 'put',
        data: {
          clockRecordId: this.staffClock.clockRecordId,
          afternoonClock: new Date(),
        }
      }).then(response => {
        console.log(response)
        if (response.data.data === "成功") {
          ElMessage.success("打卡成功")
          this.card()
        } else {
          ElMessage.error("打卡失败")
        }

      }).catch(function (error) {
        console.log(error);
      });
    }
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
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', '星期日']
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
  , created() {
    this.selectById(1);

    // 分页查询
    this.card();

  }
}


</script>