<!--历史花名册页面-->
<template>
	<div class="saas-main-content">
		<div class="j-card j-card-bordered mainContent">
			<div class="j-card-body ">
				<span></span>
			     <br/>
				<!--搜索输入框-->
        <div style="margin-top:10px;">
          <!--搜索输入框-->
          <div style="display: inline-block">
            <span style="font-weight:bold;margin-left: 30px">员工名称</span>
            <el-row style="width:200px;display: inline-block;margin-left: 15px">
              <el-input v-model="pageInfo.staffNameSearch" placeholder="请输入用户名称"/>
            </el-row>
          </div>

          <div style="display: inline-block;margin-left:25px">
            <span style="font-weight:bold">部门 </span>

            <el-select v-model="pageInfo.deptSearch" placeholder="请输入部门名称" style="width: 200px;margin-left: 15px">
              <el-option
                  v-for="item in deptNameAll"
                  :key="item.deptId"
                  :label="item.deptName"
                  :value="item.deptId"
              >
              </el-option>
            </el-select>

          </div>

          <div style="display: inline-block;margin-left:25px">
            <span style="font-weight:bold">职位 </span>

            <el-select v-model="pageInfo.postSearch" placeholder="请输入职位名称" style="width: 200px;margin-left: 15px">
              <el-option
                  v-for="item in positionAll"
                  :key="item.positionId"
                  :label="item.positionName"
                  :value="item.positionId"
              >
              </el-option>
            </el-select>

          </div>


          <el-button @click="selectHistorical()" type="primary" style="width: 70px;margin-left:25px">
            <el-icon>
              <i-search/>
            </el-icon>
            搜索
          </el-button>
          <el-button style="width: 70px;" @click="replacement()">
            <el-icon>
              <i-refresh/>
            </el-icon>
            重置
          </el-button>
        </div>
				<!-- 表格内容部分 -->
				<div class="sub-Content__primary">
					
					 <el-table :data="tableData" stripe style="width: 100%;margin-top:20px"
                     :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                     :cell-style="{textAlign: 'center'}"
           >
                        <el-table-column prop="staffName" fixed label="姓名" width="120" />
                        <el-table-column prop="deptName" label="部门" width="150" />
                        <el-table-column prop="positionName" label="职位" width="150" />
                        <el-table-column prop="staffPhone" label="手机号" width="150" />
                        <el-table-column prop="staffEmail" label="邮箱" width="150" />
                        <el-table-column prop="staffWorkingYears" label="工龄" width="120" />
                        <el-table-column prop="staffHiredate" label="入职日期" width="150" />
                        <el-table-column prop="formalQuitDate" label="离职日期" width="150" />
                        <el-table-column prop="quitExplain" label="离职原因" />
                    </el-table>
					
				</div>

        <!-- 分页插件 -->
        <div class="demo-pagination-block">
          <el-pagination
              v-model:currenPage="pageInfo.currentPage"
              :page-sizes="[3, 5, 10, 50]"
              v-model:page-size="pageInfo.pagesize"
              :default-page-size="pageInfo.pagesize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="pageInfo.total"
              :pager-count="5"
              background
              @size-change="selectHistorical"
              @current-change="selectHistorical"
          >
          </el-pagination>
        </div>



      </div>
		</div>
	</div>
</template>

<script>
	export default {
    data(){
      return{
        //存储部门名称
        deptNameAll:[],
        //存储职位名称
        positionAll:[],
          input3:"",
        pageInfo: {
          // 分页参数
          currenPage: 1, //当前页
          pagesize: 3, // 页大小
          total: 0, // 总页数

          // 员工名称
          staffNameSearch: '',
          // 部门名称
          deptSearch: '',
          //部门职位名称
          postSearch:'',
        },
      }
    },methods:{
      //搜索框重置
      replacement() {
        this.pageInfo.currentPage = 1,
            this.pageInfo.staffNameSearch = '',
            this.pageInfo.deptSearch = '',
            this.pageInfo.postSearch = '',

        this.selectHistorical()

      },

      //历史花名册 查询状态为离职的员工信息
      selectHistorical() {
        this.axios
            .get("http://localhost:8010/provider/staff/selectHistorical",{params: this.pageInfo})
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
      //查询部门名称
      selectDeptName() {
        this.axios
            .get("http://localhost:8010/provider/staff/selectDeptName")
            .then((response) => {
              console.log(response);
              this.deptNameAll = response.data.data;

            })
            .catch(function (error) {
              console.log(error);
            });
      },
      //查询职位名称
      selectPositionName() {
        this.axios
            .get("http://localhost:8010/provider/staff/selectPositionName")
            .then((response) => {
              console.log(response);
              this.positionAll = response.data.data;

            })
            .catch(function (error) {
              console.log(error);
            });
      },
    },created() {
      this.selectHistorical()
      this.selectDeptName()
      this.selectPositionName()
    }
  }

</script>


<style scoped>
@import url("../../css/navigation.css");
	/deep/.mainContent .sub-Content__primary {
	    padding: 12px 24px;
	    background: #fff;
	    border-radius: 4px;
	}

  /deep/.cell {
    padding-left: 10px;
    text-align: center;
    color: black;
  }
  /* 分页的样式 */
 /deep/ .demo-pagination-block {
    float: right;
    margin: 20px;
  }

</style>
