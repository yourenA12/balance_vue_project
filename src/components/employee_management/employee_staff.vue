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
      <div style="display: inline-block">
    <span style="font-weight:bold">员工名称</span>
        <el-row style="width:200px;display: inline-block;margin-left: 15px">
        <el-input v-model="staffNameSS" placeholder="请输入用户名称" />
      </el-row>
      </div>

    <div style="display: inline-block;margin-left:25px">
    <span style="font-weight:bold">部门 </span>
      <el-row style="width:200px;display: inline-block;margin-left:15px">
        <el-input v-model="deptSS" placeholder="请输入部门名称" />
      </el-row>
    </div>

    <div style="display: inline-block;margin-left:25px">
    <span style="font-weight:bold"> 状态</span>
      <el-form-item style="width:200px;display: inline-block;margin-left:15px">
        <el-select  placeholder="请选择状态" v-model="stateSS">
          <el-option label="正式" value="正式" style="margin-left: 15px"></el-option>
          <el-option label="试用" value="试用" style="margin-left: 15px"></el-option>
        </el-select>
      </el-form-item>
    </div>
      <div style="display: inline-block;margin-left:25px">
      <span style="font-weight:bold">入职日期</span>
        <el-row style="width:200px;display: inline-block;margin-left:15px">
        <el-input v-model="hiredateSS" placeholder="请输入日期" />
      </el-row>
      </div>
      <el-button type="primary" style="width: 70px;margin-left:25px"><el-icon><i-search /></el-icon>搜索 </el-button>
      <el-button style="width: 70px;" @click="replacement()"><el-icon><i-refresh /></el-icon>重置 </el-button>
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
        <el-table-column prop="workerDate" label="转正日期" width="150">

        </el-table-column>
        <el-table-column prop="staffWorkingYears" label="工龄" width="150"/>
        <el-table-column fixed="right" label="操作" width="150">
          <template #default="scope">
              <el-button type="text" size="small" @click="empMsg(scope.row.staffId)"
              >编辑
              </el-button>
            <!--                <router-link :to="{path:this.leave,query:{path: this.$route.query.path}}" style="text-decoration: none">-->
            &nbsp;
            <el-button @click="departure(scope.row)" type="text" size="small">办理离职</el-button>
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

    <!-- 员工办理离职弹出框 -->
    <div style=" width:100%;">
        <el-form
            ref="departure_data"
            :model="departure_data"
            :rules="departure_rules"
            label-width="120px"
            class="demo-ruleForm"
        >
          <div style="width:50%;display: inline-block;margin-left: -10px">
            <el-form-item label="名称" prop="name"  >
              <el-input v-model="departure_data.name" disabled style="width:240px"></el-input>
            </el-form-item><br/>

            <el-form-item label="离职原因" prop="cause" >
              <el-select
                  v-model="departure_data.cause"
                  placeholder="请选择" style="width:240px;"
              >
                <el-option label="家庭原因" value="家庭原因" style="margin-left: 20px"></el-option>
                <el-option label="实习生返校" value="实习生返校" style="margin-left: 20px"></el-option>
                <el-option label="回校深造" value="回校深造" style="margin-left: 20px"></el-option>
                <el-option label="交通不便" value="交通不便" style="margin-left: 20px"></el-option>
                <el-option label="身体健康因素" value="身体健康因素" style="margin-left: 20px"></el-option>
                <el-option label="薪资原因" value="薪资原因" style="margin-left: 20px"></el-option>
                <el-option label="福利原因" value="福利原因" style="margin-left: 20px"></el-option>
                <el-option label="个人发展原因" value="个人发展原因" style="margin-left: 20px"></el-option>
                <el-option label="人际关系不融洽" value="人际关系不融洽" style="margin-left: 20px"></el-option>
                <el-option label="工作环境不适应" value="工作环境不适应" style="margin-left: 20px"></el-option>
                <el-option label="团队气氛不适应" value="团队气氛不适应" style="margin-left: 20px"></el-option>
                <el-option label="企业文化不适应" value="企业文化不适应" style="margin-left: 20px"></el-option>
                <el-option label="工作职责及目标不明确" value="工作职责及目标不明确" style="margin-left: 20px"></el-option>
                <el-option label="未得到充分的支持和授权" value="未得到充分的支持和授权" style="margin-left: 20px"></el-option>
                <el-option label="其它" value="其它" style="margin-left: 20px"></el-option>
              </el-select>
            </el-form-item><br/>

            <el-form-item label="备注"  prop="remark">
              <el-input v-model="departure_data.remark" type="textarea" style="width: 240px;"></el-input>
            </el-form-item>
          </div>

          <div style="width:50%;display: inline-block;position: relative;top:-60px;">
          <el-form-item label="状态" >
            <el-input v-model="fromStaffState" disabled style="width:240px"/>
          </el-form-item><br/>



            <el-form-item label="离职生效时间" required >
              <el-col :span="11">
                <el-form-item prop="dimisiondate" style="width: 240px;">
                  <el-date-picker
                      v-model="departure_data.dimisiondate"
                      type="date"
                      placeholder="请选择日期"
                      style="width: 100%"
                  ></el-date-picker>
                </el-form-item>
              </el-col>
            </el-form-item>


          </div>
          <div style="margin-left: 300px;margin-top: 30px;">

            <el-button style="width: 80px;" @click="become=false">取消</el-button>
            <el-button style="width: 80px" type="primary" @click="Dimission(),become=false">提交
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
      //
      departure_data: {
        id:"",
        name: "",
        state: "",
        cause: "",
        workdate: '',
        dimisiondate: '',
        remark: "",
        deptId:"",
      },
      departure_rules: {
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
      deptPostId0:[],
      dpetpostid:{},
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
      },
      //搜索绑定值
      staffNameSS:'',
      deptSS:'',
      stateSS:'',
      hiredateSS:'',


      dimissionVal:null,
      staffVal:null,

      // 表单中的员工状态
      fromStaffState:"",

    }
  },
  methods: {
    //搜索框重置
    replacement(){
      this.staffNameSS='',
      this.deptSS='',
      this.stateSS='',
      this.hiredateSS=''
    },
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
    //根据id查询员工信息
    empMsg(staffId) {
      // 将当前行的员工id 存入 store里面，使得在其他页面也能取到
      this.$store.state.staffId_Msg=staffId;
      // 跳转页面
      this.$router.push({path:this.staffedit,query:{path: this.$route.query.path}})
    },
    //

    //办理离职 获取员工信息 赋值到弹出框
    departure(row) {

      //赋值到弹出框中的员工状态
      if( row.staffState==0 )
        this.fromStaffState="在职"

      if( row.staffState==1 )
        this.fromStaffState="离职"

      if( row.staffState==2 )
        this.fromStaffState="试用"

      if( row.staffState==3 )
        this.fromStaffState="正式"

      // 开启弹出框
      this.become=true;

      // 赋值到弹出框
      this.departure_data.id=row.staffId
      this.departure_data.name=row.staffName
      this.departure_data.state=row.staffState

      this.departure_data.deptId=row.deptId

    },
    //离职前调用，取值方法
    Dimission(){
      //获取离职表信息
      this.dimissionVal={
        //员工编号
        staffId:this.departure_data.id,
        //员工名称
        staffName: this.departure_data.name,
        //部门id
        deptId:this.departure_data.deptId,
        //离职原因
        quitType:this.departure_data.cause,
        //离职生效时间
        formalQuitDate:this.departure_data.dimisiondate,
        //备注
        quitExplain:this.departure_data.remark,
        //状态
        quitState:1,
      }

      //修改员工状态
      this.staffVal={
        staffState:1,
      }
      this.insertDimission()

    },  //添加离职表
    insertDimission(){
      this.axios({
        url: 'http://localhost:8010/provider/quit_Staff/insertquit_staff',
        method: 'post',
        data:{
          Staff:this.staffVal,
          Quit:this.dimissionVal,

        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.selectStaff() // 修改完成后调用查询方法
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
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