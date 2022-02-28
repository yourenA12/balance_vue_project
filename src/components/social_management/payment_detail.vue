<template>
<!--社保缴费明细-->
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body">
        <!-- 计薪月份 -->
        <div class="month-div">
          <span class="month_span">2021-12</span><br /><br />
          计薪月份<br /><br />
          <el-button type="primary" size="small">重新核算</el-button>
          <el-button type="primary" size="small" style="width: 80px"
            >归档</el-button
          >
        </div>

        <!-- 月金额统计 -->
        <div class="month_sum">
          <div>
            <span>参保人数</span><br/>
            <span>{{ insuredPeople }}</span><br/><br/><br/>
          </div>

          <div>
            <span>个人缴费</span><br/>
            <span>{{ personPay }}</span><br/><br/><br/>
          </div>

          <div>
            <span>企业缴费</span><br/>
            <span>{{ firmPay }}</span><br/><br/><br/>
          </div>

          <div>
            <span>合计缴费</span><br/>
            <span>{{ totalPay }}</span><br/><br/><br/>
          </div>
        </div>

        <!-- 社保总表 -->
        <!-- 表格按钮部分 -->
        <div class="mt-20 ml-20 mr-20">
          <!-- 按钮 -->
          <el-button size="small"
            ><i class="iconfont">&#xe6a2;</i>批量导出</el-button
          >
          <el-button size="small"
            ><i class="iconfont">&#xe639;</i>批量导入</el-button
          >
          <el-button size="small" type="danger" plain
            ><i class="iconfont">&#xe608;</i>批量删除</el-button
          >

<!--          &lt;!&ndash; 输入框 &ndash;&gt;-->
<!--          <div class="resume-operation">-->
<!--            <el-input v-model="empName_search" placeholder="员工姓名">-->
<!--              <template #suffix>-->
<!--                <el-icon class="el-input__icon"><i-search /></el-icon>-->
<!--              </template>-->
<!--            </el-input>-->
<!--          </div>-->

<!--          &lt;!&ndash; 下拉选择器 &ndash;&gt;-->
<!--          <div class="resume-operation">-->
<!--            <el-select clearable size="small" v-model="empState" placeholder="员工状态">-->
<!--              <el-option-->
<!--                v-for="item in empState_options"-->
<!--                :key="item.value"-->
<!--                :label="item.label"-->
<!--                :value="item.value"-->
<!--              >-->
<!--              </el-option>-->
<!--            </el-select>-->
<!--          </div>-->

<!--          &lt;!&ndash; 部门 树形选择框 &ndash;&gt;-->
<!--          <div class="resume-operation">-->
<!--            <el-select clearable v-model="dept_name" multiple placeholder="选择部门">-->
<!--              <el-option-->
<!--                v-for="item in depts"-->
<!--                :key="item.value"-->
<!--                :label="item.label"-->
<!--                :value="item.value"-->
<!--              >-->
<!--              </el-option>-->
<!--            </el-select>-->
<!--          </div>-->


          <el-input style="width: 200px;margin-left: 150px;" size="small" v-model="pageInfo.staffNameSearch" placeholder="请输入用户名称"/>

          <!-- 下拉选择器 -->
          <!--          <div style="width: 200px" class="resume-operation">-->

          <el-select v-model="deptId" size="small" multiple ref="vueSelect" @change="onchange()" @click="onclicks()"  placeholder="选择部门" style="margin-left: 15px">
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

          <el-select placeholder="请选择状态" size="small" v-model="pageInfo.stateSearch" style="margin-left: 15px;">
            <el-option label="正式" value="3" style="margin-left: 15px"></el-option>
            <el-option label="试用" value="2" style="margin-left: 15px"></el-option>
          </el-select>
          <!--          </div>-->

          <el-button @click="selectAllPage()" size="small" type="primary" style="width: 80px;margin-left:25px;margin-top: 20px">
            <el-icon>
              <i-search/>
            </el-icon>
            搜索
          </el-button>
          <el-button @click="replacement()" size="small"  style="width: 80px;" >
            <el-icon>
              <i-refresh/>
            </el-icon>
            重置
          </el-button>
        </div>

        <!-- 表格内容部分 -->
        <div class="sub-Content__primary">
          <el-table size="small" :data="tableData" style="width: 100%"
                    :header-cell-style="{textAlign: 'center',background:'#f0f0f0',color:'#6C6C6C'}"
                    :cell-style="{textAlign: 'center'}">
            <!-- 多选框 -->
            <el-table-column type="selection" width="55"/>
            <el-table-column prop="staffName" label="姓名"/>
            <el-table-column prop="defInsuredName" label="参保方案" />
            <el-table-column prop="insDetailSocialPersonPay" label="社保个人缴费" />
            <el-table-column prop="insDetailSocialFirmPay" label="社保企业缴费" />
            <el-table-column prop="insDetailFundPersonPay" label="公积金个人缴费" />
            <el-table-column prop="insDetailFundFirmPay" label="公积金企业缴费"/>
            <el-table-column label="操作">
              <template #default>
                <router-link :to="{path:this.path,query:{path:this.$route.query.path}}">
                  <el-button type="text" size="small">查看 </el-button>
                </router-link>
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
            @size-change="selectAllPage"
            @current-change="selectAllPage"
          >
          </el-pagination>
        </div>
      </div>
    </div>
  </div>
  &nbsp;
</template>

<script>
import { ref, defineComponent } from "vue";
import {ElMessage} from "element-plus";
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


      path:"/social/social_payment/someone_insured_particulars",
      // 部门名称
      dept_name: null,
      // 参保人数
      insuredPeople:0,
      // 个人缴费
      personPay:0,
      // 公司缴费
      firmPay:0,
      // 合计缴费
      totalPay:0,
      // 选择部门 下拉选择器
      depts: [
        {value: "1", label: "部门1"},
        {value: "2", label: "部门2"},
        {value: "3", label: "部门3"},
      ],
      // 员工姓名搜索框
      empName_search:"",
      //员工状态下拉选择器
      empState_options: [
        { value: "0", label: "试用期" },
        { value: "1", label: "在职" },
        { value: "2", label: "离职" },
      ],
      empState:"",//员工状态下拉选择器的值
      // 分页参数
      pageInfo: {
        currenPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数

        // 员工名称
        staffNameSearch: '',
        // 部门名称
        deptSearch: '',
        //员工状态
        stateSearch:'',
      },
      tableData: [],
    };
  },
  methods:{
    //搜索框重置
    replacement() {
      this.pageInfo.currentPage = 1,
          this.pageInfo.staffNameSearch = '',
          this.pageInfo.deptSearch = '',
          this.res2=""
      this.pageInfo.stateSearch='',
          // 将值赋值到选择器中
          this.$refs.tree.setCheckedKeys([], false)


      this.selectAllPage()

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

    // 查询所有参保方案
    selectAllPage() {
      let params= {

        currenPage:this.pageInfo.currenPage,
        pagesize:this.pageInfo.pagesize,
        staffNameSearch: this.pageInfo.staffNameSearch,
        deptIds:this.res2.length==0?'':this.res2,
        stateSearch: this.pageInfo.stateSearch,

      }

      this.axios
          .get("http://localhost:8010/provider/insuredDetail/selectInsuredDetail?"+qs.stringify(params,{ arrayFormat: 'repeat' }))
          .then((response) => {
            console.log(response);
            // 表格数据
            this.tableData = response.data.data.records

            // 本月参保人数
            this.insuredPeople=response.data.data.total

            /* 点击搜索会变*/

            // 遍历表格数据
            this.tableData.forEach(item=>{
              // 本月个人缴费
              this.personPay+=item.insDetailSocialPersonPay+item.insDetailFundPersonPay
              // 本月公司缴费
              this.firmPay+=item.insDetailSocialFirmPay+item.insDetailFundFirmPay
            })

            // 合计缴费
            this.totalPay=this.personPay+this.firmPay
          })
          .catch(function (error) {
            console.log(error);
          });
    },
  },
  created() {
    this.selectAllPage()
    this.selectDeptName()
  }
};
</script>

<style scoped>

/*表格*/
.sub-Content__primary{
  margin-top: 20px;
}

/* 月统计金额下的div下的span */
.month_sum div span {
  margin: 35px;
}

/* 月统计金额下的div */
.month_sum div {
  display: inline-block;
}

/* 月金额统计 */
.month_sum {
  display: inline-block;
  margin-left: 100px;
}

/* 计薪月份div */
.month-div {
  display: inline-block;
  margin: 40px;
  margin-left: 100px;
}

/* 计薪月份 */
.month_span {
  font-weight: bold;
  font-size: 30px;
}

/* 调整输入框的高度 */
::v-deep .el-input__inner {
  height: 32px;
}

/* 分页的样式 */
.demo-pagination-block {
  float: right;
  margin: 20px;
}

/**
	 * 下拉选择器样式
	 */
.resume-operation {
  padding-left: 8px;
  float: right;
  width: 120px;
}

/* 外层阴影 */
.saas-main-content {
  padding-top: 12px;
  min-height: 500px;
}

.j-card-bordered {
  border: 1px solid #e9e9e9;
}

.j-card {
  background: #fff;
  border-radius: 4px;
  font-size: 14px;
  position: relative;
  overflow: hidden;
  transition: all 0.3s;
  margin-top: 8px;
  min-height: 100%;
}
.j-card:hover {
  box-shadow: 0 1px 6px rgba(0, 0, 0, 0.2);
  border-color: transparent;
}
.j-card-bordered {
  border: 1px solid #e9e9e9;
  border-top-color: rgb(233, 233, 233);
  border-right-color: rgb(233, 233, 233);
  border-bottom-color: rgb(233, 233, 233);
  border-left-color: rgb(233, 233, 233);
}
.j-card {
  background: #fff;
  border-radius: 4px;
  font-size: 14px;
  position: relative;
  overflow: hidden;
  transition: all 0.3s;
  margin-top: 8px;
  min-height: 100%;
}

.j-card-body{
  padding:0 2%;
}

@font-face {
  font-family: 'iconfont';  /* Project id 2990377 */
  src: url('//at.alicdn.com/t/font_2990377_1cwdx3n4atr.woff2?t=1639139822080') format('woff2'),
  url('//at.alicdn.com/t/font_2990377_1cwdx3n4atr.woff?t=1639139822080') format('woff'),
  url('//at.alicdn.com/t/font_2990377_1cwdx3n4atr.ttf?t=1639139822080') format('truetype');
}

</style>

