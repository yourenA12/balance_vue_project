<!--调动管理页面-->
<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body">
        <span></span>
        <br />
        <div style="width:95%;background: #e8edf2;padding-top: 30px;margin: auto;" v-show="changesadd">
          <div style="width: 90%;margin: auto;">
            <h2 >
              <span>人事异动</span>
              -
              <span>添加</span>
            </h2>
            <div style="width: 80%;margin: auto;margin-top: 30px;position: relative;padding-bottom:30px ">

              <el-form :model="mobilizeForm" :rules="rules" ref="mobilizeForm" label-width="100px" class="demo-ruleForm">
                <div style="display: inline-block;">

                  <el-form-item label="姓名：" prop="name">
                    <div class="name_tb" >
                      <span style="margin-left: 10px;">{{mobilizeForm.staffName}}</span>
                      <span @click="become = true"><i  class="iconfont" style="position: absolute;left:210px; cursor:pointer;">&#xe629;</i></span>
                    </div>
                  </el-form-item><br/>

                  <el-form-item label="调动前部门:" prop="dept">
                    <el-select v-model="mobilizeForm.createdDeptId" placeholder="请选择惩罚类型" style="width: 240px;" disabled>
                      <el-option
                          v-for="item in deptNameAll"
                          :key="item.deptId"
                          :label="item.deptName"
                          :value="item.deptId"
                      >
                      </el-option>

                    </el-select>
                  </el-form-item><br/>


                  <el-form-item label="调动前职位:" prop="position">
                    <el-select v-model="mobilizeForm.transferRawpostId" placeholder="请选择惩罚类型" style="width: 240px;" disabled>
                      <el-option
                          v-for="item in deptPostNameAll"
                          :key="item.deptPostId"
                          :label="item.postName"
                          :value="item.deptPostId"
                      >
                      </el-option>

                    </el-select>
                  </el-form-item><br/>



                  <el-form-item label="生效日期：" prop="takeEffectDate">
                    <el-col :span="11">
                      <el-date-picker type="date" placeholder="选择日期" v-model="mobilizeForm.takeEffectDate" style="width: 240px;"></el-date-picker>
                    </el-col>
                  </el-form-item>
                </div>
                <div style="display: inline-block;position: absolute;left:500px;" >
                  <el-form-item label="异动类型：" prop="type">
                    <el-select v-model="mobilizeForm.transferType" placeholder="请选择活动区域" style="width:240px;">
                      <el-option label="调岗" value="调岗" style="margin-left: 20px;"></el-option>
                      <el-option label="普升" value="普升" style="margin-left: 20px;"></el-option>
                      <el-option label="降职" value="降职" style="margin-left: 20px;"></el-option>
                    </el-select>
                  </el-form-item><br/>

                  <el-form-item label="调动后部门:" prop="dept">
                    <el-select v-model="mobilizeForm.deptId" placeholder="请选择惩罚类型" style="width: 240px;">
                      <el-option
                          v-for="item in deptNameAll"
                          :key="item.deptId"
                          :label="item.deptName"
                          :value="item.deptId"
                      >
                      </el-option>

                    </el-select>
                  </el-form-item><br/>


                  <el-form-item label="调动后职位:" prop="position">
                    <el-select v-model="mobilizeForm.postId" placeholder="请选择惩罚类型" style="width: 240px;">
                      <el-option
                          v-for="item in deptPostNameAll"
                          :key="item.deptPostId"
                          :label="item.postName"
                          :value="item.deptPostId"
                      >
                      </el-option>

                    </el-select>
                  </el-form-item><br/>


                </div>

                <div style="width:25%;height: 50px;margin: auto;margin-top: 20px;">
                  <el-button @click="changesadd=false" style="width: 80px;">取消</el-button>
                  <el-button  type="primary" style="width: 80px;" @click="transfer_staff()">提交</el-button>
                </div>

              </el-form>
            </div>
          </div>

        </div>
        <div style="width:100%;height: 100px;position: relative  ">

<!--          添加按钮-->
           <el-button @click="changesadd=true,selectDeptName(),selectDeptPostName()" size="small" type="primary" plain style="width:80px;position: absolute;top:30px;left:30px">
          <el-icon><i-plus/></el-icon>
          新增
        </el-button>




          <!--搜索输入框-->
          <el-row style="width: 200px;position: absolute;left:1090px;top:30px;">
            <el-input
                      v-model="seek" placeholder="搜索">
              <template #suffix @click="become = true">
                <el-icon class="el-input__icon"><i-search /></el-icon>
              </template>
            </el-input>
          </el-row>
        </div>

        <!-- 表格内容部分 -->
        <div class="sub-Content__primary">
          <el-table :data="tableData"
                    :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                    :cell-style="{textAlign: 'center'}">
            <el-table-column prop="staffName" label="姓名" width="180"/>
            <el-table-column prop="transferType" label="异动类型" width="180"/>
            <el-table-column prop="deptName" label="原部门" width="180"/>
            <el-table-column prop="deptName2" label="变动后部门" width="180"/>
            <el-table-column prop="postName" label="原职位" width="180"/>
            <el-table-column prop="postName2" label="变动后职位" width="180"/>
            <el-table-column prop="takeEffectDate" label="生效时间" width="180" />
<!--            <el-table-column fixed="right" label="操作" width="120">-->
<!--              <template #default="scope">-->
<!--                <el-button type="text" size="small" >撤销 </el-button>-->
<!--                <el-button type="text" size="small" @click="TransferXX(scope.row)">编辑 </el-button>-->
<!--              </template>-->
<!--            </el-table-column>-->
          </el-table>
        </div>
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
            @size-change="selectTransfer()"
            @current-change="selectTransfer()"
        >
        </el-pagination>
      </div>

      <div>
        <el-dialog
            v-model="become"
            title="选择员工"
            width="50%"
            :close-on-click-modal="false">

          <!--搜索输入框-->
          <el-row style="width: 200px;margin-left:528px;">
            <el-input v-model="seek2" placeholder="搜索">
              <template #suffix @click="become = true">
                <el-icon class="el-input__icon"><i-search /></el-icon>
              </template>
            </el-input>
          </el-row>

          <el-table
              :data="deptData"
              height="250"
              style="width: 100%;margin-top: 20px;"
              :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
              :cell-style="{textAlign: 'center'}">

            <el-table-column width="60">
              <template #default="scope">
                <el-radio :label="scope.row.staffId" v-model="radioStaff" @change.native="getCurrentRow(scope.row)">
                  <!-- 以为有Label的原因，所以添加&nbsp以空格显示 -->
                  &nbsp;
                </el-radio>
              </template>

            </el-table-column>

            <el-table-column
                prop="staffName"
                label="姓名"
                width="180">
            </el-table-column>
            <el-table-column
                prop="deptName"
                label="部门"
                width="180">
            </el-table-column>
            <el-table-column
                prop="postName"
                label="职位">
            </el-table-column>
          </el-table>

          <div style="margin-top: 30px;margin-left:280px">
            <el-button @click="become=false" style="width: 80px;">取消</el-button>
            <el-button type="primary" style="width: 80px;" @click="staffRow()">确定</el-button></div>

        </el-dialog>
      </div>

    </div>
  </div>
</template>

<script>
// import {ref} from "vue/dist/vue";
import { defineComponent, ref } from 'vue'
import {ElMessage} from "element-plus";
export default defineComponent({
  data(){
    const one = (rule, value, callback) => {
      if (new Date()>value){
        callback(new Error("生效日期不能小于当前时间"));
      }else{
        callback();
      }

    };
    return{
      //部门名称
      deptNameAll:[],
      //部门职位
      deptPostNameAll:[],
      // 单选框选择员工
      radioStaff:"",
      // 单选框选中的员工信息
      information:{},
      become:false,

      seek:"",
      seek2:'',
      //隐藏input文本框
      changesadd:false,
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
      },
      mobilizeForm: {
        //员工编号
        staffId:'',
        //员工姓名
        staffName: '',
        //异动类型
        transferType:'',
        //调动前部门
        createdDeptId:'',
        //调动后部门
        deptId:'',
        //调动前职位
        transferRawpostId:'',
        //调动后职位
        postId:'',
        //生效日职
        takeEffectDate:'',

      },
      rules: {
        // tableData: [
        //   {
        //     required: true,
        //     message: '请选择生效日期',
        //     trigger: 'change',
        //   },
        //   {
        //     validator: one, trigger: "change"
        //   }
        // ],
        type:[
          {
            required:true,
            message:'请选择异动类型',
            trigger:'bulr',
          }
        ]
      },
      radio:"",
      //弹出框存储数据
      deptData: [],
      //调动记录
      transferVal:null,
      staffVal:null
    }
  },
  methods: {

    //获取单选框选中数据 (选中某个员工进行‘调动‘)
    getCurrentRow(row){
      this.information=row
    },
    //把单选框里的值点出来
    staffRow(){
      //员工id
      this.mobilizeForm.staffId=this.information.staffId;
      //员工姓名
      this.mobilizeForm.staffName=this.information.staffName;
      //调动前部门
      this.mobilizeForm.createdDeptId=this.information.deptId;
      //调动前职位
      this.mobilizeForm.transferRawpostId=this.information.deptPostId;

      this.become=false;

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
    //查询部门职位名称
    selectDeptPostName() {
      this.axios
          .get("http://localhost:8010/provider/staff/selectDeptPostName")
          .then((response) => {
            console.log(response);
            this.deptPostNameAll = response.data.data;

          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //查询调动记录
    selectTransfer() {
      this.axios
          .get("http://localhost:8010/provider/transfer/selectTransfer/"+this.pageInfo.currentPage+"/"+this.pageInfo.pagesize)
          .then((response) => {
            console.log(response);
            this.tableData = response.data.data.records
            console.log(response.data.data.records)
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //添加调动记录表和修改员工职位、部门
    transfer_staff(){
      //添加调动记录
      this.transferVal={
        //员工id
        staffId:this.mobilizeForm.staffId,
        //异动类型
        transferType:this.mobilizeForm.transferType,
        //原部门名称
        createdDeptId:this.mobilizeForm.createdDeptId,
        //状态
        transferState:1,
        //调动后部门名称
        updatedDeptId:this.mobilizeForm.deptId,
        //原职位名称
        transferRawpostId:this.mobilizeForm.transferRawpostId,
        //调动后职位名称
        transferAfterpostId:this.mobilizeForm.postId,
        //生效日期
        takeEffectDate:this.mobilizeForm.takeEffectDate,
      }

      //修改员工职位、部门
      this.staffVal={
        //员工id
        staffId:this.mobilizeForm.staffId,
        //职位编号
         deptPostId:this.mobilizeForm.postId,
        //部门编号
        deptId: this.mobilizeForm.deptId,
      }

      //添加调动记录表
      this.insertTransfer()

    },
    //添加调动记录表
    insertTransfer(){

      this.axios({
        url: 'http://localhost:8010/provider/transfer/insertTransfer',
        method: 'post',
        data:{
          Transfer:this.transferVal,
          Staff:this.staffVal
        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.changesadd=false // 添加完成关闭input框
          this.selectTransfer();
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
        console.log(error);
      });

    },
    //把表格里的值赋在input文本上
    TransferXX(row){
      this.changesadd=true;
      this.mobilizeForm.staffName=row.staffName;
      this.mobilizeForm.createdDeptId=row.deptName;
      this.mobilizeForm.transferRawpostId=row.postName;
      this.mobilizeForm.transferType=row.transferType;
      this.mobilizeForm.takeEffectDate=row.takeEffectDate;
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    // RestForm(){
    //   this.ruleForm= {
    //     name: '',
    //         formerdept: '',
    //         formerpost: '',
    //         takedate: '',
    //         type: '',
    //         transferdept: '',
    //         transferpost: ''
    //   }
    // },//多表查询员工信息
    selectStaffXX() {
      this.axios
          .get("http://localhost:8010/provider/staff/selectStaffXX")
          .then((response) => {
            console.log(response);
            this.deptData = response.data.data;
          })
          .catch(function (error) {
            console.log(error);
          });
    },

  },created() {
    this.selectStaffXX();
    this.selectTransfer();
  },



})
</script>


<style scoped>
/*@import url("../../css/navigation.css");*/
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
/deep/.demo-pagination-block {
  float: right;
  margin: 20px;
}

.name_tb{
  width:240px;
  height:40px;
  border:1px solid silver;
  border-radius: 3px;
  background: white;
  position: relative;
}

.iconfont {
  font-family: "iconfont" !important;
  font-size: 20px;
  font-style: normal;
  color: #5aaaff;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}


@font-face {
  font-family: 'iconfont';  /* Project id 2982823 */
  src: url('//at.alicdn.com/t/font_2982823_fi6h9uqcpu6.woff2?t=1639378694122') format('woff2'),
  url('//at.alicdn.com/t/font_2982823_fi6h9uqcpu6.woff?t=1639378694122') format('woff'),
  url('//at.alicdn.com/t/font_2982823_fi6h9uqcpu6.ttf?t=1639378694122') format('truetype');
}

</style>
