<!--花名册页面-->
<template>
  <div class="saas-main-content">


    <div style="width:100%;height:100px;">
      <div class="staff_div_5">

        <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe614;</i>
        <div class="staff_div_5_text">
          <span>在职</span><br/><span>2</span>
        </div>
      </div>

      <div class="staff_div_5 div_5_margin">
        <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe60b;</i>
        <div class="staff_div_5_text">
          <span>本月离职</span><br/><span>2</span>
        </div>
      </div>

      <div class="staff_div_5 div_5_margin">
        <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe60c;</i>
        <div class="staff_div_5_text">
          <span>正式</span><br/><span>2</span>
        </div>
      </div>

      <div class="staff_div_5 div_5_margin">
        <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe61f;</i>
        <div class="staff_div_5_text">
          <span>实习</span><br/><span>2</span>
        </div>
      </div>

      <div class="staff_div_5 div_5_margin">
        <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe616;</i>
        <div class="staff_div_5_text">
          <span>本月新入职</span><br/><span>2</span>
        </div>
      </div>
    </div>


    <div style="margin-top:10px;">
      <!--搜索输入框-->
      <el-row style="width:200px;float:right;">
        <el-input v-model="input3" placeholder="搜索">
          <template #suffix>
            <el-icon class="el-input__icon">
              <i-search/>
            </el-icon>
          </template>
        </el-input>
      </el-row>
    </div>
    <br/>
    <div style="margin-top:30px;">
      <el-table :data="tableData" style="width: 100%"
                :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                :cell-style="{textAlign: 'center'}">

        <el-table-column fixed prop="staffName" label="姓名" width="150"/>
        <el-table-column prop="staffBirthday" label="出生日期" width="150"/>
        <el-table-column prop="deptName" label="部门" width="150"/>
        <el-table-column prop="postName" label="职位" width="150"/>
        <el-table-column prop="staffPhone" label="手机" width="150"/>
<!--        <el-table-column prop="staffState" label="状态" width="150">-->
<!--          <template #default="scope">-->
<!--             {{scope.row.staffState==''}}-->
<!--          </template>-->
<!--        </el-table-column>-->
        <el-table-column label="状态" width="150">
          <template #default="scope">
            <span v-if="scope.row.staffState==0">在职</span>
            <span v-if="scope.row.staffState==1">离职</span>
            <span style="color:#fa8c16;" v-if="scope.row.staffState==2">试用</span>
            <span style="color: #13c2c2;" v-if="scope.row.staffState==3">正式</span>
          </template>
        </el-table-column>
        <el-table-column prop="staffHiredate" label="入职日期" sortable width="150">
        </el-table-column>
        <el-table-column prop="state" label="转正日期" width="150">

        </el-table-column>
        <el-table-column prop="state" label="工龄" width="150"/>
        <el-table-column fixed="right" label="操作" width="150">
          <template #default>
            <router-link :to="{path:this.staffedit,query:{path: this.$route.query.path}}">
              <el-button type="text" size="small" @click="handleClick"
              >编辑
              </el-button>
            </router-link>
            <!--                <router-link :to="{path:this.leave,query:{path: this.$route.query.path}}" style="text-decoration: none">-->
            &nbsp;
            <el-button @click="become=true" type="text" size="small">办理离职</el-button>
            <!--                </router-link>-->
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
          @size-change="selectStaff"
          @current-change="selectStaff"
      >
      </el-pagination>
    </div>

  </div>

  <div>
    <el-dialog
        v-model="become"
        title="办理离职"
        width="50%"
        :close-on-click-modal="false"
    >

    <div style=" width:100%;">
        <el-form
            ref="ruleForm"
            :model="ruleForm"
            :rules="rules"
            label-width="120px"
            class="demo-ruleForm"
        >
          <div style="width:50%;display: inline-block;margin-left: -10px">
            <el-form-item label="名称" prop="name" >
              <el-input v-model="ruleForm.name" disabled style="width:240px"></el-input>
            </el-form-item><br/>

            <el-form-item label="离职原因" prop="cause" >
              <el-select
                  v-model="ruleForm.cause"
                  placeholder="请选择" style="width:240px;"
              >
                <el-option label="家庭原因" value="jtyy" style="margin-left: 20px"></el-option>
                <el-option label="实习生返校" value="sxsfx" style="margin-left: 20px"></el-option>
                <el-option label="回校深造" value="hxsz" style="margin-left: 20px"></el-option>
                <el-option label="交通不便" value="jtbb" style="margin-left: 20px"></el-option>
                <el-option label="身体健康因素" value="stjkys" style="margin-left: 20px"></el-option>
                <el-option label="薪资原因" value="xzyy" style="margin-left: 20px"></el-option>
                <el-option label="福利原因" value="flyy" style="margin-left: 20px"></el-option>
                <el-option label="个人发展原因" value="grfzyy" style="margin-left: 20px"></el-option>
                <el-option label="人际关系不融洽" value="rjgxyy" style="margin-left: 20px"></el-option>
                <el-option label="工作环境不适应" value="gzhjbsy" style="margin-left: 20px"></el-option>
                <el-option label="团队气氛不适应" value="tdqfbsy" style="margin-left: 20px"></el-option>
                <el-option label="企业文化不适应" value="qyehbsy" style="margin-left: 20px"></el-option>
                <el-option label="工作职责及目标不明确" value="gzzejmbbmq" style="margin-left: 20px"></el-option>
                <el-option label="未得到充分的支持和授权" value="wddcfdzchsq" style="margin-left: 20px"></el-option>
                <el-option label="其它" value="qt" style="margin-left: 20px"></el-option>
              </el-select>
            </el-form-item><br/>

            <el-form-item label="离职生效时间" required >
              <el-col :span="11">
                <el-form-item prop="dimisiondate" style="width: 240px;">
                  <el-date-picker
                      v-model="ruleForm.dimisiondate"
                      type="date"
                      placeholder="请选择日期"
                      style="width: 100%"
                  ></el-date-picker>
                </el-form-item>
              </el-col>
            </el-form-item>
          </div>

          <div style="width:50%;display: inline-block">
          <el-form-item label="状态" >
            <el-input v-model="ruleForm.state" disabled style="width:240px"></el-input>
          </el-form-item><br/>



          <el-form-item label="最后工作时间" required >
            <el-col :span="11">
              <el-form-item prop="workdate" style="width: 240px;">
                <el-date-picker
                    v-model="ruleForm.workdate"
                    type="date"
                    placeholder="请选择日期"
                    style="width: 100%"
                ></el-date-picker>
              </el-form-item>
            </el-col>
          </el-form-item><br/>



          <el-form-item label="备注"  prop="remark">
            <el-input v-model="ruleForm.remark" type="textarea" style="width: 240px;"></el-input>
          </el-form-item>
          </div>
          <div style="margin-left: 300px;margin-top: 30px;">

            <el-button style="width: 80px;" @click="become=false">取消</el-button>
            <el-button style="width: 80px" type="primary" @click="submitForm('ruleForm')">提交
            </el-button>
          </div>
        </el-form>
      </div>



  </el-dialog>
  </div>

</template>
<script lang="ts">
import {defineComponent, ref} from 'vue'
import { ElMessage } from "element-plus";
export default {
//操作时间

  data() {
    const value2 = ref('')
    const one = (rule, value, callback) => {
      if (new Date() > value) {
        callback(new Error("最后工作时间不能小于当前时间"));
      } else if (this.ruleForm.dimisiondate < value) {
        callback(new Error("最后工作时间不能大于离职生效时间"));
      } else {
        callback();
      }

    };
    // var date1=this.ruleForm.workdate;
    const two = (rule, value, callback) => {
      if (this.ruleForm.workdate > value) {
        callback(new Error("离职生效时间不能小于最后工作时间"));
      } else if (new Date() > value) {
        callback(new Error("离职生效时间不能小于当前时间"));
      } else {
        callback();
      }
    };
    return {
      //员工花名册
      book: '/employee/message/employee_roster/book',
      ruleForm: {
        name: "",
        state: "",
        cause: "",
        workdate: '',
        dimisiondate: '',
        remark: "",
      },
      rules: {
        cause: [
          {
            required: true,
            message: "请选择离职原因",
            trigger: "change",
          },
        ],
        workdate: [
          {
            type: "date",
            required: true,
            message: "请选择最后工作时间",
            trigger: "change",
          },
          {
            validator: one, trigger: "change"
          }
        ],
        dimisiondate: [
          {
            type: "date",
            required: true,
            message: "请选择离职生效时间",
            trigger: "change",
          },
          {
            validator: two, trigger: "change"
          }
        ],
      },


      become: false,
      leave: '/employee/message/employee_roster/leave',
      staffedit: '/employee/message/employee_roster/staffedit',
      tableData: [
      ],
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
      },
      input3: "",
      value2: "",
    }
  },
  methods: {
    //多表查询
    selectStaff() {
      this.axios
          .get("http://localhost:8010/provider/staff/selectStaffVo/"+this.pageInfo.currentPage+"/"+this.pageInfo.pagesize)
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
  }, created() {
    this.selectStaff();
  },
}
</script>
<style scoped>
@import url("../../css/navigation.css");

/deep/ .cell {
  padding-left: 10px;
  text-align: center;
  color: black;
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

.staff_div_5 {
  width: 237px;
  height: 80px;
  border: 1px solid #f0f0f0;
  display: inline-block;
  border-radius: 5px;
  position: relative;
  box-shadow: 0 1px 6px rgba(0, 0, 0, 0.2);


}

.div_5_margin {

  margin-left: 25px;
}

.staff_div_5_text {
  width: 80px;
  height: 50px;
  /*background: #5cbdaa;*/
  display: inline-block;
  position: absolute;
  left: 140px;
  top: 20px;
  text-align: right;
  font-weight: bold;
  color: #085fc3;

}

</style>