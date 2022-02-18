<!--  放弃入职员工页面  -->
<template>
  <div class="saas-main-content">

    <!--搜索输入框-->
    <div style="width:100%;height:50px;margin: auto;">
      <!--搜索输入框-->
      <div style="display: inline-block;">
        <span style="font-weight:bold;margin-left: 10px">统计</span>
        <div style="margin-left: 15px;display: inline-block;">
          <span>本月放弃入职人数:</span><span style="margin-left: 10px;color: red;">{{ thisMonth }}</span>
          <span style="margin-left: 20px;">上个月放弃入职人数:</span><span style="margin-left: 10px;color: red;">{{ lastMonth }}</span>
        </div>
      </div><br/>

      <div style="display: inline-block;margin-top:20px;">
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

      <div style="display: inline-block;margin-left:25px">
        <span style="font-weight:bold">职位 </span>

        <el-select v-model="pageInfo.postSearch" placeholder="请输入部门名称" style="width: 200px;margin-left: 15px">
          <el-option
              v-for="item in positionAll"
              :key="item.positionId"
              :label="item.positionName"
              :value="item.positionId"
          >
          </el-option>
        </el-select>

      </div>

      <el-button @click="selectAbandon()" type="primary" style="width: 80px;margin-left:25px;margin-top: 20px">
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
    <div style="width:100px;height:30px;margin-top:40px;"></div>
    <el-table :data="tableData" style="width: 100%;"
              :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
              :cell-style="{textAlign: 'center'}" >
      <el-table-column prop="resumeName" label="姓名" width="200"/>
      <el-table-column prop="deptName" label="部门" width="200"/>
      <el-table-column prop="positionName" label="职位" width="200"/>
      <el-table-column prop="resumePhone" label="手机号" width="200"/>
      <el-table-column prop="hiredate" label="入职日期" width="200"/>
      <el-table-column prop="waiveReason" label="放弃原因" width=""/>

    </el-table>

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
          @size-change="selectAbandon"
          @current-change="selectAbandon"
      >
      </el-pagination>
    </div>


  </div>
</template>

<script>
import qs from "qs";

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



      //存储职位信息
      positionAll:"",
      tableData: [],
      input3: '',
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
      // 本月入职
      thisMonth:0,
      // 上月入职
      lastMonth:0,


    }
  },methods:{

    //搜索框重置
    replacement() {
      this.pageInfo.currentPage = 1,
          this.pageInfo.staffNameSearch = '',
          this.res2=""
      // 将值赋值到选择器中
      this.$refs.tree.setCheckedKeys([], false)
          this.pageInfo.postSearch = '',


      this.selectAbandon()

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
    //多表查询
    selectAbandon() {

      let params= {

        currenPage:this.pageInfo.currenPage,
        pagesize:this.pageInfo.pagesize,
        staffNameSearch: this.pageInfo.staffNameSearch,
        deptIds:this.res2.length==0?'':this.res2,
        postSearch:this.pageInfo.postSearch,
      }
      this.axios
          .get("http://localhost:8010/provider/entryhirdeVo/selectEntryhirdeVoFQ?"+qs.stringify(params,{ arrayFormat: 'repeat' }))
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
    //查询本月放弃入职员工人数
    selectFqDate(type) {

      let yy = new Date().getFullYear();// 获取当前年份
      let mm = new Date().getMonth()+1;// 获取当前月份
      let date;
      let mm1;

      // 判断查询的当月还是上月 0是当月
      if(type==0){
        // 如果月份小于10 就在前面加个0
        mm1=mm<10?"0"+mm:mm;
        date=yy+"-"+mm1 // 拼接字符串
      }else{

        if(mm==1){// 如果当前月是1月
          mm=12 // 上个月为12
          yy-- // 当前年份减一
        }else{
          mm-- // 月份减一
        }
        // 如果月份小于10 就在前面加个0
        mm1=mm<10?"0"+mm:mm;
        date=yy+"-"+mm1

      }

      this.axios
          .get("http://localhost:8010/provider/entryhirdeVo/selectFqDate/"+date)
          .then((response) => {
            console.log(response);
            if(type==0){
              this.thisMonth=response.data.data;
            }else{
              this.lastMonth=response.data.data;
            }

          })
          .catch(function (error) {
            console.log(error);
          });
    },
  },created() {
    this.selectAbandon();
    this.selectDeptName();
    this.selectPositionName();
    this.selectFqDate(0);
    this.selectFqDate(1);

  },
}
</script>


<style scoped>
/deep/ .mainContent .sub-Content__primary {
  padding: 12px 24px;
  background: #fff;
  border-radius: 4px;
}

/deep/ .cell {
  padding-left: 10px;
  text-align: center;
  color: black;
}

.xxx{
  display: none;
}

/* 分页的样式 */
/deep/ .demo-pagination-block {

  float: right;
  margin: 20px;
}


/*.saas-main-content{*/
/*  position: relative;*/

/*}*/
</style>
