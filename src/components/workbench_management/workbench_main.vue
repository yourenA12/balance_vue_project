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
              <h3>欢迎你：{{ this.$store.state.userMsg.staffName }}，祝你工作顺利<br>
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
                <span class="font_xu">  {{ this.$store.state.userMsg.deptName }} | {{ this.$store.state.userMsg.postName }} </span></h3>

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
              <span class="right_font">薪酬管理</span>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover" class="body_col_3">
              <el-icon>
                <i-avatar class="i_icon_svg"/>
              </el-icon>
              <span class="right_font">员工管理</span>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover" class="body_col_4">
              <el-icon>
                <i-avatar class="i_icon_svg"/>
              </el-icon>
              <span class="right_font">招聘管理</span>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover" class="body_col_5">
              <el-icon>
                <i-avatar class="i_icon_svg"/>
              </el-icon>
              <span class="right_font">组织管理</span>
            </el-card>
          </el-col>



          <div class="right_float_gg">

            <el-tabs type="border-card">
              <el-tab-pane label="公司公告" >


                <div style="border-bottom: 1px solid #dddddd;"></div>

                <div v-if="tableData1.length==0" style="color: #ddd">
                  暂无未读公告
                </div>

                <div v-if="tableData1.length!=0" v-for="(item,index) in tableData1" class="noticeV" style="border-bottom: 1px solid #dddddd;">

                {{ item.noticeTitle }}

                 <el-button  class="button_left_xq" type="text" @click="drawer = true,tableItem=index,noticeId=item.noticeId">详情</el-button>

                </div>



              </el-tab-pane>
              <el-drawer v-model="drawer" :with-header="false" title="I am the title">
                <div>
                  <h1>公告内容：</h1>
                </div>
                <br>
                <div style="margin: 30px;color: red;">
                  {{ tableData1[tableItem].noticeMatter }}
                </div>
                <el-form :model="item" label-width="">
                  <el-form-item label="公告发起人 :">
                    <el-input v-model="tableData1[tableItem].staffName" disabled></el-input>
                  </el-form-item>
                  <br>
                  <el-form-item label="公告类型 :">
                    <el-input v-model="tableData1[tableItem].noticeTitle" disabled></el-input>
                  </el-form-item>
                </el-form>
                <br>
                <br>
                <el-button type="primary" @click="updateAuditflowdetai()" :disabled = "tableData1[tableItem].noticeState==0?false:true">&nbsp;&nbsp; 确定收到&nbsp;&nbsp;</el-button>
              </el-drawer>

<!--             已读公告 -->
              <el-tab-pane label="Config">
                <div style="border-bottom: 1px solid #dddddd;"></div>

                <div v-if="tableData2.length==0">
                  暂无未读公告
                </div>

                <div v-if="tableData2.length!=0" v-for="(item,index) in tableData2" class="noticeV" style="border-bottom: 1px solid #dddddd;">

               {{ item.noticeTitle }}


                <el-button class="button_left_xq" type="text" @click="drawer2 = true,tableItem2=index,noticeId=item.noticeId">详情</el-button>

              </div>


              </el-tab-pane>

            </el-tabs>
          </div>

          <el-drawer v-model="drawer2" :with-header="false" title="I am the title">
            <div>
              <h1>公告内容：</h1>
            </div>
            <br>
            <div style="margin: 30px;color: red;" >
              {{ tableData2[tableItem2].noticeMatter }}
            </div>
            <el-form :model="item" label-width="">
              <el-form-item label="公告发起人 :">
                <el-input v-model="tableData2[tableItem2].staffName" disabled></el-input>
              </el-form-item>
              <br>
              <el-form-item label="公告类型 :">
                <el-input v-model="tableData2[tableItem2].noticeTitle" disabled></el-input>
              </el-form-item>
            </el-form>
            <br>
            <br>
            <el-button type="primary" disabled>&nbsp;&nbsp; 已收到&nbsp;&nbsp;</el-button>
          </el-drawer>



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
<!--    <div class="left_data_right">
      <div class="top_data_right">
        <h3>今日提醒</h3>
      </div>
    </div>-->
    <br>


<!--    <div class="tong_left" id="main1">

    </div>-->
<!--
    <div id="main2" class="tong_left">

    </div>-->
  </div>


</template>

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
  background: #0e8f61;
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

      tableItem:0,
      tableItem2:0,
      //
      displa1:true,
      calendar,
      selectDate,
      // staffClock:{},


    }
  },
  data() {
    return {
      activeName: 'first',
      tableData1: [],
      tableData2: [],
      drawer: false,
      drawer2:false,
      id1:"",
      id: 1,
      noticeId:"",

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

    updateAuditflowdetai() {
      this.axios({
        url: 'http://localhost:8010/provider/noticeStaff/findNoticeStaffById',
        method: 'put',
        data:{
          staffId:this.$store.state.userMsg.staffId,
          noticeId:this.noticeId,
        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '操作成功',
            type: 'success',
          })
          this.selectById() // 修改完成后调用查询方法
          this.drawer=false
        } else {
          ElMessage.error('操作失败')
        }
      }).catch(function (error) {
        console.log(error);
      });
    },



    selectById() {

      //根据id查询
      this.axios
          .get("http://localhost:8010/provider/findSelectNoticeById/"+this.$store.state.userMsg.staffId)
          .then((response) => {
            console.log(response);
            this.tableData1 = response.data.data;
           /* console.error(this.tableData1)*/
          })
          .catch(function (error) {
            console.log(error);
          })
    },
    selectById1() {
      //根据id查询
      this.axios
          .get("http://localhost:8010/provider/findSelectNoticeById1/"+this.$store.state.userMsg.staffId)
          .then((response) => {
            console.log(response);
            this.tableData2 = response.data.data;
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
  created() {
    this.selectById();
    this.selectById1();
    // 分页查询
    this.card();

  }
}


</script>
