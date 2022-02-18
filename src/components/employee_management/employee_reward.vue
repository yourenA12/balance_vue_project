<!--奖罚记录-->
<template>
<!--  <div class="saas-main-content">-->
<!--    <div class="j-card j-card-bordered mainContent">-->
      <div class="j-card-body">

        <!--搜索输入框-->
        <div style="width:100%;height:50px;margin: auto;">
          <!--搜索输入框-->
          <div style="display: inline-block;margin-top:30px;">
            <span style="font-weight:bold;margin-left: 10px">员工名称</span>
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

          <el-button @click="selectGlory(),selectPunish()" type="primary" style="width: 80px;margin-left:25px;margin-top: 20px">
            <el-icon>
              <i-search/>
            </el-icon>
            搜索
          </el-button>
          <el-button @click="replacement()" style="width: 80px;" >
            <el-icon>
              <i-refresh/>
            </el-icon>
            重置
          </el-button>
        </div>
        <div style="width:100%;margin-top: 50px;" class="icon-p">


          <div style="width:100%;height:100px;margin: auto;background: #42b983;">
            <div style="text-align: center;">
              <el-table :data="tableGloryData"
                        :header-cell-style="{textAlign: 'center',background:'#f8f8f9'}"
                        :cell-style="{textAlign: 'center'}"
                        style="width: 100%;font-size: 12px;color:black;font-weight: normal;">
                <el-table-column label="荣誉与奖励">
                  <el-table-column prop="staffName" label="荣誉与奖励姓名" width="230"/>
                  <el-table-column prop="deptName" label="部门" width="230"/>
                  <el-table-column prop="createdTime" label="奖励日期" width="230"/>
                  <el-table-column prop="gloryName" label="颁发单位名称" width="230"/>
                    <el-table-column   prop="gloryRemark" label="备注" width="230" />
                </el-table-column>
                <el-table-column prop="name" label="操作" width="100" fixed="right">
                  <router-link :to="{path:this.information,query:{path: this.$route.query.path}}">
                    <el-button type="text" size="small">编辑</el-button>
                  </router-link>
                </el-table-column>
              </el-table>
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
                    @size-change="selectGlory"
                    @current-change="selectGlory"
                >
                </el-pagination>
              </div><br/><br/>


              <el-table :data="tablePunishData"
                        :header-cell-style="{textAlign: 'center',background:'#f8f8f9'}"
                        :cell-style="{textAlign: 'center'}"
                        style="width: 100%;font-size: 12px;color:black;font-weight: normal;">

                <el-table-column label="惩罚">
                  <el-table-column prop="staffName" label="惩罚人名称" width="170"/>
                  <el-table-column prop="deptName" label="部门" width="170"/>
                  <el-table-column prop="punishType" label="惩罚类型" width="170"/>
                  <el-table-column prop="punishCause" label=" 惩罚原因" width="190"/>
                  <el-table-column prop="punishUnit" label="惩罚单位" width="150"/>
                  <el-table-column prop="isRevocation" label=" 是否撤销" width="140"/>
                  <el-table-column prop="punishRemark" label="备注" width="180"/>
                </el-table-column>


                <el-table-column prop="name" label="操作" width="100" fixed="right">
                  <router-link :to="{path:this.information,query:{path: this.$route.query.path}}">
                  <el-button type="text" size="small">编辑</el-button>
                  </router-link>
                </el-table-column>
              </el-table>

            </div>

            <!-- 分页插件 -->
            <div class="demo-pagination-block">
              <el-pagination
                  v-model:currentPage="pageInfo1.currentPage"
                  :page-sizes="[3, 5, 10, 50]"
                  v-model:page-size="pageInfo1.pagesize"
                  :default-page-size="pageInfo1.pagesize"
                  layout="total, sizes, prev, pager, next, jumper"
                  :total="pageInfo1.total"
                  :pager-count="5"
                  background
                  @size-change="selectPunish"
                  @current-change="selectPunish"
              >
              </el-pagination>
            </div>
          </div>
        </div>
      </div>
<!--    </div>-->
<!--  </div>-->

</template>


<script>
import qs from 'qs'
export default {
  data() {
    // 格式
    const defaultProps = {
      children: 'children',
      label: 'deptName',
      value:'deptId'
    }
    return {

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


      information:'/employee/message/employee_roster/information',
      tableGloryData: [],
      tablePunishData: [],
      seek: "",
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数


        // 员工名称
        staffNameSearch: '',
        // 部门名称
        deptSearch: '',

      },

      pageInfo1: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数


        // 员工名称
        staffNameSearch: '',
        // 部门名称
        deptSearch: '',

      },

    }
  },methods:{
    // 重置
    replacement(){
      this.pageInfo.staffNameSearch=""
      this.res2=""
      // 将值赋值到选择器中
      this.$refs.tree.setCheckedKeys([], false)

      this.selectGlory()
      this.selectPunish()

    },
    //分页查询奖励表
    selectGlory() {

      let params= {

        currenPage:this.pageInfo.currentPage,
        pagesize:this.pageInfo.pagesize,
        staffNameSearch: this.pageInfo.staffNameSearch,
        deptIds:this.res2.length==0?'':this.res2,

      }

      this.axios
          .get("http://localhost:8010/provider/Glory/selectGloryPage?"+qs.stringify(params,{ arrayFormat: 'repeat' }))
          .then((response) => {
            console.log(response);
            this.tableGloryData = response.data.data.records;
            console.log(response.data.data.records)
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //分页查询惩罚表
    selectPunish() {

      let params= {

        currenPage:this.pageInfo1.currentPage,
        pagesize:this.pageInfo1.pagesize,
        staffNameSearch: this.pageInfo.staffNameSearch,
        deptIds:this.res2

      }

      this.axios
          .get("http://localhost:8010/provider/Punish/selectPunishPage?"+qs.stringify(params,{ arrayFormat: 'repeat' }))
          .then((response) => {
            console.log(response);
            this.tablePunishData = response.data.data.records;
            console.log(response.data.data.records)
            this.pageInfo1.total = response.data.data.total;
          })
          .catch(function (error) {
            console.log(error);
          });
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
  },created() {
    this.selectGlory()
    this.selectPunish()
    this.selectDeptName()
  }
}
</script>
<style scoped>
@import url("../../css/navigation.css");

.xxx{
  display: none;
}

/deep/ .el-card__body {
  /* padding: var(--el-card-padding); */
  padding: 10px 0;
}


/deep/ .icon-p p {
  text-align: center;
}

/* 分页的样式 */
/deep/ .demo-pagination-block {
  float: right;
  margin: 20px;
}

</style>
