
<!--工资表 -->
<template>
        <div>
          <el-tabs type="card" @tab-click="handleClick">

<!--            &lt;!&ndash;月份选择框·&ndash;&gt;-->
<!--            <div style="display: inline-block;margin-left: 840px;margin-top:20px">-->
<!--              <div class="demo-date-picker">-->
<!--                <div class="container">-->
<!--                  <div class="block">-->
<!--                    <span class="demonstration"></span>-->

<!--                    <el-date-picker-->
<!--                        v-model="months"-->
<!--                        type="month"-->
<!--                        placeholder="请选择月份"-->
<!--                    >-->
<!--                    </el-date-picker>-->
<!--                  </div>-->
<!--                </div>-->
<!--              </div>-->
<!--            </div>-->





            <el-tab-pane label="未归档">

              <div style="margin-top:30px;margin-left: 890px">
                <!--搜索输入框-->
                <div style="display: inline-block">
                  <el-row style="width:200px;display: inline-block;margin-left: 15px">
                    <el-input v-model="pageInfo.seek" placeholder="请输入薪酬组名称"/>
                  </el-row>
                </div>





                <el-button @click="selectWagenotfiledVo()" type="primary" style="width: 80px;margin-left:25px">
                  <el-icon>
                    <i-search/>
                  </el-icon>
                  搜索
                </el-button>
                <el-button style="width: 80px;" @click="replacement()">
                  <el-icon>
                    <i-refresh/>
                  </el-icon>
                  重置
                </el-button>
              </div><br/>
              <!-- 表格内容部分 -->
              <div class="sub-Content__primary">
                <el-table :data="tableData"  style="width: 98%;margin:auto"
                          :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                          :cell-style="{textAlign: 'center'}">
                  <el-table-column prop="compensationName" label="薪酬组"  />
                  <el-table-column prop="wagenotfiledAskperson" label="计薪人数"/>
                  <el-table-column prop="wagenotfiledSalary" label="应发工资" />
                  <el-table-column prop="wagenotfiledPayrollsalary" label="实发工资"  />
                  <el-table-column prop="wagenotfiledFirmpayment" label="公司缴纳"  />
                  <el-table-column prop="wagenotfiledState" label="状态">
                    <template #default="scope">
                      <span style="color:#fa8c16;" v-if="scope.row.wagenotfiledState==0">未归档</span>
                      <span style="color: #13c2c2;" v-if="scope.row.wagenotfiledState==1">已归档</span>

                    </template>
                  </el-table-column>
                  <el-table-column  label="操作" >
                    <template #default="scope">
                      <router-link :to="{path:this.wagesheet,query:{path: this.$route.query.path,compId:scope.row.compensationId}}"><el-button type="text" size="small" @click="handleClick">查看工资表&nbsp;</el-button></router-link>
<!--                      <el-button type="text" size="small" @click="handleClick">归档</el-button>-->
                      <el-popconfirm title="归档之后，数据一律不允许修改" @confirm="updatewagenotfiledVo(scope.row)">
                        <template #reference>
                          <el-button type="text" size="small">归档 </el-button>
                        </template>
                      </el-popconfirm>
                    </template>
                  </el-table-column>

                </el-table>
              </div>

              <!-- 分页插件 -->
              <div class="demo-pagination-block">
                <el-pagination
                    v-model:currentPage="pageInfo.currenPage"
                    :page-sizes="[3, 5, 10, 50]"
                    v-model:page-size="pageInfo.pagesize"
                    :default-page-size="pageInfo.pagesize"
                    layout="total, sizes, prev, pager, next, jumper"
                    :total="pageInfo.total"
                    :pager-count="5"
                    background
                    @size-change="selectWagenotfiledVo"
                    @current-change="selectWagenotfiledVo"
                >
                </el-pagination>
              </div>


            </el-tab-pane>





            <el-tab-pane label="已归档">



              <div style="margin-top:30px;margin-left: 890px">
                <!--搜索输入框-->
                <div style="display: inline-block">
<!--                  <span style="font-weight:bold;margin-left: 30px">员工名称</span>-->
                  <el-row style="width:200px;display: inline-block;margin-left: 15px">
                    <el-input v-model="pageInfo1.seek" placeholder="请输入薪酬组名称"/>
                  </el-row>
                </div>





                <el-button @click="selectWagenotfiledVos()" type="primary" style="width: 80px;margin-left:25px">
                  <el-icon>
                    <i-search/>
                  </el-icon>
                  搜索
                </el-button>
                <el-button style="width: 80px;" @click="replacements()">
                  <el-icon>
                    <i-refresh/>
                  </el-icon>
                  重置
                </el-button>
              </div><br/>

              <!-- 表格内容部分 -->
              <div class="sub-Content__primary">

                <el-table :data="tableDatas" stripe style="width: 98%;margin: auto"
                          :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                          :cell-style="{textAlign: 'center'}">
                  <el-table-column prop="compensationName" label="薪酬组"  />
                  <el-table-column prop="wagenotfiledAskperson" label="计薪人数"/>
                  <el-table-column prop="wagenotfiledSalary" label="应发工资" />
                  <el-table-column prop="wagenotfiledPayrollsalary" label="实发工资"  />
                  <el-table-column prop="wagenotfiledFirmpayment" label="公司缴纳"  />
                  <el-table-column prop="wagenotfiledState" label="状态">
                    <template #default="scope">
                      <span style="color:#fa8c16;" v-if="scope.row.wagenotfiledState==0">未归档</span>
                      <span style="color: #13c2c2;" v-if="scope.row.wagenotfiledState==1">已归档</span>

                    </template>
                  </el-table-column>
                  <el-table-column  label="操作" width="170">
                    <template #default>
                      <router-link :to="{path:this.wagesheet,query:{path: this.$route.query.path}}"><el-button type="text" size="small" @click="handleClick">查看工资表&nbsp;</el-button></router-link>
                    </template>
                  </el-table-column>
                </el-table>
              </div>

              <!-- 分页插件 -->
              <div class="demo-pagination-block">
                <el-pagination
                    v-model:currentPage="pageInfo1.currenPage"
                    :page-sizes="[3, 5, 10, 50]"
                    v-model:page-size="pageInfo1.pagesize"
                    :default-page-size="pageInfo1.pagesize"
                    layout="total, sizes, prev, pager, next, jumper"
                    :total="pageInfo1.total"
                    :pager-count="5"
                    background
                    @size-change="selectWagenotfiledVos"
                    @current-change="selectWagenotfiledVos"
                >
                </el-pagination>
              </div>

            </el-tab-pane>
          </el-tabs>
        </div>



</template>
<script lang="ts">
import {ElMessage} from "element-plus/es";

export default {

  data() {
    return {
      //工资表
      // wagesheet:'/salary/wagesheet',
      wagesheet:'/salary/selectwagetable',

      //存未归档数据
      tableData: [],
      tableDatas:[],
      months:'',
      seek:'',
      pageInfo: {
        // 分页参数
        currenPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数

        //薪酬组
        seek:'',
      },

      pageInfo1: {
        // 分页参数
        currenPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数

        //薪酬组
        seek:'',
      },




    }

  },
  methods:{
    //搜索框重置
    replacement() {
      this.pageInfo.currenPage = 1
      this.pageInfo.seek = ''

      this.selectWagenotfiledVo()

    },
    replacements() {
      this.pageInfo1.currenPage = 1
      this.pageInfo1.seek = ''

      this.selectWagenotfiledVos()

    },

    //查询未归档数据
    selectWagenotfiledVo(){

      this.axios
          .get("http://localhost:8010/provider/wagenotfiled/selectwagenotfiledVo", {params:this.pageInfo})
          .then((response) => {
            console.log(response);
            this.tableData = response.data.data.records;
            console.log(response.data.data.records)
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //修改未归档未已归档
    updatewagenotfiledVo(row){

      this.axios({
        url: 'http://localhost:8010/provider/wagenotfiled/updateWagenotfiledVo',
        method: 'put',
        data: {
          wagenotfiledId:row.wagenotfiledId,
          wagenotfiledState:1
        }
      }).then(response => {
        console.log(response)
        if (response.data.data >0) {
          ElMessage({
            message: '修改成功',
            type: 'success',
          })
          //调用查询工作经历
          this.selectWagenotfiledVo()

        } else {
          ElMessage.error('修改失败')
        }
      }).catch(function (error) {
        console.log(error);
      });
    },
    //查询归档数据
    selectWagenotfiledVos(){

      this.axios
          .get("http://localhost:8010/provider/wagenotfiled/selectwagenotfiledVos", {params:this.pageInfo1})
          .then((response) => {
            console.log(response);
            this.tableDatas = response.data.data.records;
            console.log(response.data.data.records)
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error) {
            console.log(error);
          });
    },
  },created() {
    this.selectWagenotfiledVo()
    this.selectWagenotfiledVos()
  }

}

</script>
<style scoped>
@import url("../../css/Salary.css");
.demo-pagination-block + .demo-pagination-block {
  margin-top: 10px;
}
/deep/.cell {
  padding-left: 10px;
  text-align: center;
  color: black;
}
/* 分页的样式 */
.demo-pagination-block {
  float: right;
  margin: 20px;
}

</style>
