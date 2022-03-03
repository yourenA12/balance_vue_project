<!--考勤方案 -->
<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body">
        <span></span>
        <div style="position: relative;top:35px">
          <router-link :to="{path:this.insertattendanceplan,query:{path: this.$route.query.path,name:'新增'}}">
            <button type="button" class="ant-btn ant-btn-primary">
              <i aria-label="图标: plus" class="anticon anticon-plus">
                <svg viewBox="64 64 896 896" focusable="false" class="" data-icon="plus" width="1em" height="1em" fill="currentColor" aria-hidden="true"><path d="M482 152h60q8 0 8 8v704q0 8-8 8h-60q-8 0-8-8V160q0-8 8-8z"></path>
                  <path d="M176 474h672q8 0 8 8v60q0 8-8 8H176q-8 0-8-8v-60q0-8 8-8z"></path>
                </svg></i>
              <span>新增</span>
            </button>
          </router-link>
        </div>
        <div style="width: 97%;margin: auto">
          <!--搜索输入框-->
          <el-row style="width:200px;float:right;">
            <el-input v-model="seek" placeholder="方案名称">
              <template #suffix>
                <el-icon class="el-input__icon"><i-search/></el-icon>
              </template>
            </el-input>
          </el-row>
        </div>

        <!-- 表格内容部分 -->
        <div class="sub-Content__primary" style="margin-top: 70px">
          <el-table :data="tableData"
                    :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                    :cell-style="{textAlign: 'center'}"
                    style="width: 97%;margin: auto;">
            <el-table-column prop="attendandceName" label="方案名称" width="260" />
            <el-table-column prop="name" label="核算规则" width="290" >
              <template #default="scope">
                <span>迟到：扣{{scope.row.attendandceLitemoney}}元 x 迟到次数</span><br/>
                <span>早退：扣{{scope.row.attendandceLeavemoney}}元 x 早退次数</span><br/>
                <span>旷工：小时工资x{{scope.row.attendandceAbscntmoney}}% x 旷工折算的小时数</span><br/>


              </template>
            </el-table-column>
            <el-table-column prop="attendandceRemark" label="备注" width="240" />
            <el-table-column prop="attendandceState" label="状态" width="250" >
              <template #default="scope">
                <span v-if="scope.row.attendandceState==0" style="color: #5aaaff">启用</span>
                <span v-if="scope.row.attendandceState==1" style="color: red">禁用</span>

              </template>
            </el-table-column>
            <el-table-column  label="操作" width="250">
              <template #default="scope">
                                 <el-button type="text"  size="small" @click="AttendandceMsg(scope.row.attendandceId)">编辑 </el-button>

                <el-button type="text" size="small" @click="updateAttendandceState(scope.row.attendandceId,scope.row.attendandceState)">
                  {{ scope.row.attendandceState === 0 ? '禁用 ' : '启用 ' }}
                </el-button>
<!--                <el-button type="text" size="small">删除 </el-button>-->
                <el-popconfirm
                    confirm-button-text="确定"
                    cancel-button-text="取消"
                    title="确认要删除此方案吗?"
                    @confirm="deleteAttendandceId(scope.row)"
                >
                  <template #reference>
                    <el-button type="text" size="small" style="color: orange">删除 </el-button>
                  </template>
                </el-popconfirm>

              </template>
            </el-table-column>
          </el-table>

        </div>

        <!-- 分页插件 -->
        <div class="demo-pagination-block">
          <el-pagination
              v-model:currentPage="pageInfo.currentPage"
              :page-sizes="[3, 5, 10, 50]"
              v-model:page-size="pageInfo.pagesize"
              :default-page-size="pageInfo.pagesize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="pageInfo.total"
              :pager-count="5"
              background
              @size-change="selectAttendandce"
              @current-change="selectAttendandce"
          >
          </el-pagination>
        </div>



      </div>
    </div>
  </div>
</template>

<script>
import {ElMessage} from "element-plus";

export default {
  data(){
    return{
      //新增编辑考勤扣款方案
      insertattendanceplan:'/salary/insertattendanceplan',
      seek:"",
      tableData: [
        {
          date: '2016-05-03',
          name: 'Tom',
          state: 'California',
          city: 'Los Angeles',
          address: 'No. 189, Grove St, Los Angeles',
          zip: 'CA 90036',
          tag: 'Home',
        },
        {
          date: '2016-05-02',
          name: 'Tom',
          state: 'California',
          city: 'Los Angeles',
          address: 'No. 189, Grove St, Los Angeles',
          zip: 'CA 90036',
          tag: 'Office',
        },
      ],
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
      },

    }
  },
  methods:{
    // 删除行
    deleteRow(index, rows) {
      rows.splice(index, 1);
      ElMessage({
        message: '删除成功',
        type: 'success',
      })
    },
    //查询考勤扣款方案
    selectAttendandce() {

      this.axios
          .get("http://localhost:8010/provider/attendandce/selectAttendandce/"+this.pageInfo.currentPage+"/"+this.pageInfo.pagesize)
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
    //修改考勤扣款方案状态
    updateAttendandceState(id,state){
      this.axios
          .put(" http://localhost:8010/provider/attendandce/updateAttendandceState",{attendandceId:id,attendandceState:state==0?1:0})
          .then((response) => {
            console.log(response);
            if(response.data.data>0){
              ElMessage({
                type: 'success',
                message: '操作成功！！',
              })
              // 调用查询
              this.selectAttendandce()
            }else{
              ElMessage('操作失败！！')
            }
          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //删除考勤扣款方案
    deleteAttendandceId(index){
      this.axios
          .delete("http://localhost:8010/provider/attendandce/deleteAttendandceId/"+index.attendandceId)
          .then((response) => {
            console.log(response);

            if (response.data.data >0) {
              ElMessage({
                message: '删除成功',
                type: 'success',
              })
              //调用查询
              this.selectAttendandce()


            } else {
              ElMessage.error('删除失败')
            }
          })
          .catch(function (error) {
            console.log(error);
          });
    },
    AttendandceMsg(row){
      //跳转页面
      this.$router.push({path:this.insertattendanceplan,query:{path: this.$route.query.path,name:'编辑',id:row}})


    }
  },created() {
    this.selectAttendandce();
  }
}
</script>


<style scoped>
@import url("../../css/navigation.css");
/deep/.ant-btn-primary {
  color: #fff;
  background-color: #085fc3;
  border-color: #085fc3;
  box-shadow: 0 2px 0 rgb(0 0 0 / 5%);
  margin-left: 20px;
}
/deep/.cell {
  padding-left: 10px;
  text-align: center;
  color: black;
}

/* 分页的样式 */
/deep/.demo-pagination-block {
  float: right;
  margin: 20px;
}

</style>