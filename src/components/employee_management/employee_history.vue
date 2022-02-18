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

            <el-select v-model="deptId" multiple ref="vueSelect" @change="onchange()" @click="onclicks()">
              <el-option hidden></el-option>
              <el-option
                  class="xxx"
                  v-for="item in dept"
                  :key="item.deptId"
                  :label="item.deptName"
                  :value="item.deptId"
              >
              </el-option>
              <el-tree :data="deptlists"
                       show-checkbox
                       :default-expand-all=true
                       :check-on-click-node=true
                       node-key="deptId"
                       :props="defaultProps" ref="tree" @check-change="handleCheckChange()" />
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
	import qs from "qs";

  export default {
    data(){
      // 格式
      const defaultProps = {
        children: 'children',
        label: 'deptName',
        value:'deptId'
      }
      return{

        res:"",
        // 选中值1
        res1:"",
        // 选中值2
        res2:"",
        // 部门  文本框的值
        dept:[],
        deptId:[],
        // 格式
        defaultProps,
        //存放部门信息
        deptlists: [],


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
            this.res2=""
        // 将值赋值到选择器中
        this.$refs.tree.setCheckedKeys([], false)
            this.pageInfo.postSearch = '',

        this.selectHistorical()

      },

      // 当文本框值发生变化时调用的方法
      onchange(){

        // 将值赋值到选择器中
        this.$refs.tree.setCheckedKeys(this.deptId, false)
      },

      // 点击文本框时调用的方法
      onclicks() {

        // 取当前选择器中的复选框选项id
        this.res1 = this.$refs.tree.getCheckedKeys()
      },

      //节点选中状态发生变化时调用的方法
      handleCheckChange(data, checked, indeterminate) {

        //获取所有选中的节点 start
        this.res = this.$refs.tree.getCheckedNodes()

        // 取当前选择器中的复选框选项id
        this.res2 = this.$refs.tree.getCheckedKeys()
        // 清空部门
        this.dept = []
        // 清空选中的部门
        this.deptId = []
        let x = 0
        for (let i = 0; i < this.res.length; i++) {

          for (let j = 0; j < this.res.length; j++) {
            // 如果父id 不等于 id 就加入到数据中
            if (this.res[i].deptPid != this.res[j].deptId) {
              //并且是最后一个
              if (j == this.res.length - 1 && x == 0) {
                // 加入数据
                this.dept.push(this.res[i])
                // 赋值到文本框
                this.deptId.push(this.res[i].deptId)
              }

            } else {
              x = 1
            }
          }
          x = 0
        }
      },

      //历史花名册 查询状态为离职的员工信息
      selectHistorical() {
        let params= {

          currenPage:this.pageInfo.currenPage,
          pagesize:this.pageInfo.pagesize,
          staffNameSearch: this.pageInfo.staffNameSearch,
          deptIds:this.res2.length==0?'':this.res2,
          postSearch:this.pageInfo.postSearch,
        }

        this.axios
            .get("http://localhost:8010/provider/staff/selectHistorical?"+qs.stringify(params,{ arrayFormat: 'repeat' }))
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
            .get("http://localhost:8010/provider/dept/selectAll")
            .then((response) => {
              console.log(response);
              this.deptlists = response.data.data;

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

.xxx{
  display: none;
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
