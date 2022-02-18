<template>
<!-- 新增修改方案 -->
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body">
        <!-- 标题 -->
        <div class="big-title">&nbsp;&nbsp;&nbsp;{{ this.$route.query.name }}参保方案</div>

        <!-- 参保方案名称输入框 -->
        <div class="all-input">
          方案名称：&nbsp;
          <el-input v-model="schemeName" placeholder="输入方案名称"/>
        </div>
        <!-- 标题 -->
        <div class="big-title">&nbsp;&nbsp;&nbsp;社保方案设置</div>

        <!-- 社保方案设置，表单 -->
        <div class="all-input">
          <span style="font-weight: bold">参缴社保</span><br/>
          社保基数范围：
          <el-input-number
              v-model="security_cardinal_lower"
              size="small"
              :min="0"
              controls-position="right"
              placeholder="基数下限"
              style="width: 120px"
          />
          &nbsp; ~ &nbsp;
          <el-input-number
              v-model="security_cardinal_upper"
              size="small"
              :min="0"
              controls-position="right"
              placeholder="基数上限"
              style="width: 120px"
          />
          &nbsp;
          <!-- 弹出消息 -->
          <el-popover
              placement="right-start"
              title="基数范围"
              :width="200"
              trigger="hover"
              content="不填写基数上限，默认 无上限"
          >
            <template #reference>
              <el-button type="text"
              ><i class="iconfont">&#xe600;</i></el-button
              >
            </template>
          </el-popover>
          <br/>

          <!-- 社保缴纳项目，表单 -->
          <el-table :data="social_tableData" max-height="464" style="width: 100%">
            <!-- 缴纳项目 -->
            <el-table-column fixed label="缴纳项目" width="150">
              <template #default="scope">
                <el-select size="small" v-model="scope.row.defSchemeType" filterable placeholder="Select">
                  <el-option
                      v-for="item in social_options"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                  >
                  </el-option>
                </el-select>
              </template>
            </el-table-column>
            <!-- 基数范围 -->
            <el-table-column label="基数范围" width="300">
              <template #default="scope">
                <el-input-number
                    v-model="scope.row.defSchemeFloor"
                    size="small"
                    :min="0"
                    controls-position="right"
                    placeholder="基数下限"
                    style="width: 120px"
                />
                &nbsp; ~ &nbsp;
                <el-input-number
                    v-model="scope.row.defSchemeUpper"
                    size="small"
                    :min="0"
                    controls-position="right"
                    placeholder="基数上限"
                    style="width: 120px"
                />
              </template>
            </el-table-column>
            <!-- 公司缴纳比例 -->
            <el-table-column label="公司缴纳比例" width="180">
              <template #default="scope">
                <el-input-number
                    v-model="scope.row.defSchemeFirmProp"
                    size="small"
                    :min="0"
                    controls-position="right"
                    style="width: 120px"
                />&nbsp;&nbsp;%
              </template>
            </el-table-column>
            <!-- 个人缴纳比例 -->
            <el-table-column label="个人缴纳比例" width="180">
              <template #default="scope">
                <el-input-number
                    v-model="scope.row.defSchemePersonProp"
                    size="small"
                    :min="0"
                    controls-position="right"
                    style="width: 120px"
                />&nbsp;&nbsp;%
              </template>
            </el-table-column>
            <!-- 公司固定金额 -->
            <el-table-column label="公司固定金额" width="150">
              <template #default="scope">
                <el-input-number
                    v-model="scope.row.defSchemeFirmSum"
                    size="small"
                    :min="0"
                    controls-position="right"
                    style="width: 120px"
                />
              </template>
            </el-table-column>
            <!-- 个人固定金额 -->
            <el-table-column prop="zip" label="个人固定金额" width="150">
              <template #default="scope">
                <el-input-number
                    v-model="scope.row.defSchemePersonSum"
                    size="small"
                    :min="0"
                    controls-position="right"
                    style="width: 120px"
                />
              </template>
            </el-table-column>
            <!-- 操作 -->
            <el-table-column label="操作" width="100">
              <template #default="scope">
                <el-button @click.prevent="deleteRow(scope.$index, social_tableData)" type="text" size="small">删除
                </el-button>
              </template>
            </el-table-column>
          </el-table>

          <br/>
          <!-- 新增社保缴纳项目 -->
          <el-button @click="add_social()" size="small" type="primary" plain>+添加缴纳项目</el-button>
        </div>
        <!-- 标题 -->
        <div class="big-title">&nbsp;&nbsp;&nbsp;公积金方案设置</div>

        <!-- 公积金方案设置，表单 -->
        <div class="all-input">
          <el-table :data="accumulation_tableData" max-height="236" style="width: 100%">
            <!-- 缴纳项目 -->
            <el-table-column fixed label="缴纳项目" width="150">
              <template #default="scope">
                <el-select size="small" v-model="scope.row.defSchemeType" filterable placeholder="Select">
                  <el-option
                      v-for="item in accumulation_options"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                  >
                  </el-option>
                </el-select>
              </template>
            </el-table-column>
            <!-- 基数范围 -->
            <el-table-column label="基数范围" width="300">
              <template #default="scope">
                <el-input-number
                    v-model="scope.row.defSchemeFloor"
                    size="small"
                    :min="0"
                    controls-position="right"
                    placeholder="基数下限"
                    style="width: 120px"
                />
                &nbsp; ~ &nbsp;
                <el-input-number
                    v-model="scope.row.defSchemeUpper"
                    size="small"
                    :min="0"
                    controls-position="right"
                    placeholder="基数上限"
                    style="width: 120px"
                />
              </template>
            </el-table-column>
            <!-- 公司缴纳比例 -->
            <el-table-column label="公司缴纳比例" width="180">
              <template #default="scope">
                <el-input-number
                    v-model="scope.row.defSchemeFirmProp"
                    size="small"
                    :min="0"
                    controls-position="right"
                    style="width: 120px"
                />&nbsp;&nbsp;%
              </template>
            </el-table-column>
            <!-- 个人缴纳比例 -->
            <el-table-column prop="city" label="个人缴纳比例" width="180">
              <template #default="scope">
                <el-input-number
                    v-model="scope.row.defSchemePersonProp"
                    size="small"
                    :min="0"
                    controls-position="right"
                    style="width: 120px"
                />&nbsp;&nbsp;%
              </template>
            </el-table-column>
            <!-- 公司固定金额 -->
            <el-table-column prop="address" label="公司固定金额" width="150">
              <template #default="scope">
                <el-input-number
                    v-model="scope.row.defSchemeFirmSum"
                    size="small"
                    :min="0"
                    controls-position="right"
                    style="width: 120px"
                />
              </template>
            </el-table-column>
            <!-- 个人固定金额 -->
            <el-table-column prop="zip" label="个人固定金额" width="150">
              <template #default="scope">
                <el-input-number
                    v-model="scope.row.defSchemePersonSum"
                    size="small"
                    :min="0"
                    controls-position="right"
                    style="width: 120px"
                />
              </template>
            </el-table-column>
            <!-- 操作 -->
            <el-table-column label="操作" width="100">
              <template #default="scope">
                <el-button @click.prevent="deleteRow(scope.$index, accumulation_tableData)" type="text" size="small">
                  删除
                </el-button>
              </template>
            </el-table-column>
          </el-table>
          <br/>
          <!-- 新增公积金缴纳项目 -->
          <el-button @click="add_accumulation()" size="small" type="primary" plain>+添加缴纳项目</el-button>
        </div>

        <!-- 标题 -->
        <div class="big-title">&nbsp;&nbsp;&nbsp;选择参保组织</div>

        <div>
          <el-form ref="ruleFormRef"
                   :model="compensationForm"
                   :rules="rules"
                   label-width="120px"
                   class="demo-ruleForm"
                   :size="formSize"
          >

            <el-form-item label="试用部门：" style="margin-top: 20px;">
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
            </el-form-item>

            <el-form-item label="试用职位：" style="margin-top: 20px;">
              <el-select  placeholder="请选择试用职位" multiple v-model="compensationForm.citysPost" style="width:240px">
                <el-option
                    v-for="item in positionAll"
                    :key="item.positionId"
                    :label="item.positionName"
                    :value="item.positionId" />
              </el-select>
            </el-form-item>

            <el-form-item label="适用员工：" style="margin-top: 20px;">

              <el-select ref="vueSelect" @click="become=true,staffSelect()" v-model="compensationForm.staffs"  placeholder="请选择适用员工" multiple style="width:240px">
                <el-option
                    class="xxx"
                    v-for="item in tableVal"
                    :key="item.staffId"
                    :label="item.staffName"
                    :value="item.staffId" />
              </el-select>

            </el-form-item>

          </el-form>
        </div>

        <!-- 标题 -->
        <div class="big-title">&nbsp;&nbsp;&nbsp;计算验证</div>

        <!-- 缴费计算表单 -->
        <div class="all-input">
          <el-input-number
              v-model="security_cardinal"
              size="small"
              :min="0"
              controls-position="right"
              placeholder="请输入社保基数"
              style="width: 180px"
          />
          &nbsp;&nbsp;&nbsp;&nbsp;
          <el-input-number
              v-model="accumulation_cardinal"
              size="small"
              :min="0"
              controls-position="right"
              placeholder="请输入公积金基数"
              style="width: 180px"
          />&nbsp;&nbsp;&nbsp;
          <el-button type="primary" size="small">计算</el-button>

          <!-- 弹出消息 -->
          <el-popover
              placement="right-start"
              title="结果四舍五入到分"
              :width="200"
              trigger="hover"
              content="如计算结果数额不对请检查以上基数比例及取整规则是否正确"
          >
            <template #reference>
              <el-button type="text"
              ><i class="iconfont">&#xe600;</i></el-button
              >
            </template>
          </el-popover>
          <br/>
          个人缴纳：<span style="color: #ff9515">0</span>元&nbsp;&nbsp;&nbsp;
          公司缴纳：<span style="color: #ff9515">0</span>元
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
          <el-button type="text" @click="payment_detail=!payment_detail">缴纳明细表</el-button>

          <br/><br/>

          <!-- 缴纳明细表 -->
          <el-table v-show="payment_detail" :data="payment_details_table" style="width: 99.1%">
            <el-table-column prop="defSchemeType" label="缴纳项目"/>
            <el-table-column prop="cardinal_number" label="基数"/>

            <el-table-column label="公司缴纳">
              <el-table-column prop="defSchemeFirmProp" label="缴纳比例"/>
              <el-table-column prop="company_payment_money" label="缴纳金额"/>
            </el-table-column>

            <el-table-column prop="defSchemeFirmSum" label="公司固定金额"/>

            <el-table-column label="个人缴纳">
              <el-table-column prop="defSchemePersonProp" label="缴纳比例"/>
              <el-table-column prop="personage_payment_money" label="缴纳金额"/>
            </el-table-column>

            <el-table-column prop="defSchemePersonSum" label="个人固定金额"/>

            <el-table-column prop="date" label="小计"/>
          </el-table>

        </div>
        <div style="text-align: center">
          <router-link :to="{path:this.path,query:{path:this.$route.query.path}}">
            <el-button size="small">取消</el-button>
          </router-link>
          &nbsp;&nbsp;
          <el-button @click="verify()" size="small" type="primary">保存</el-button>
        </div>
      </div>

    </div>
  </div>

  <div>
    <el-dialog
        v-model="become"
        title="选择员工"
        width="50%">

      <!--          &lt;!&ndash;搜索输入框&ndash;&gt;-->
      <!--          <el-row style="width: 200px;margin-left:528px;">-->
      <!--          请选择一个部门:<el-input v-model="seek2" placeholder="搜索">-->
      <!--              <template #suffix @click="become = true">-->
      <!--                <el-icon class="el-input__icon"><i-search /></el-icon>-->
      <!--              </template>-->
      <!--            </el-input>-->
      <!--          </el-row>-->

      <div style="display: inline-block;margin-left: 490px">
        <span style="font-weight:bold">部门 </span>

        <!--            <el-select v-model="pageInfo.deptSearch" placeholder="请输入部门名称" style="width: 200px;">-->
        <!--              <el-option-->
        <!--                  v-for="item in positionAll"-->
        <!--                  :key="item.positionId"-->
        <!--                  :label="item.positionName"-->
        <!--                  :value="item.positionId"-->
        <!--              >-->
        <!--              </el-option>-->
        <!--            </el-select>-->

      </div>

                <el-table
                    :data="staffData"
                    @selection-change="staffAll"
                    height="250"
                    style="width: 100%;margin-top: 20px;"
                    :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                    :cell-style="{textAlign: 'center'}">

                  <!-- 全选操作按钮 -->
                  <el-table-column type="selection" width="90" />

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
                      prop="positionName"
                      label="职位">
                  </el-table-column>
                </el-table>

<!--                <div style="margin-top: 30px;margin-left:280px">-->
<!--                  <el-button @click="become=false" style="width: 80px;">取消</el-button>-->
<!--                  <el-button type="primary" style="width: 80px;" @click="staffRow()">确定</el-button>-->
<!--                </div>-->

    </el-dialog>
  </div>

  &nbsp;
</template>

<script>
import {ref, defineComponent} from "vue";
import {ElMessage} from "element-plus";

export default {
  methods: {

    verify(){

      // 验证参保方案名称
      if(this.schemeName=="" || this.schemeName==null ){
        ElMessage({
          message: '参保方案名称不能为空！！',
          type: 'warning',
        })
        return
      }

      if( this.security_cardinal_lower >= this.security_cardinal_upper && this.security_cardinal_upper!=0 ){
        ElMessage({
          message: '基数上限要大于基数下限！！',
          type: 'warning',
        })
        return
      }

      for( let i=0; i<this.social_tableData.length; i++ ){

        for( let j=0; j<this.social_tableData.length; j++ ){
            if( this.social_tableData[i].defSchemeType == this.social_tableData[j].defSchemeType && i!=j ){
              ElMessage({
                message: '险种名称重复！！',
                type: 'warning',
              })
              return
            }
        }

      }

      for( let i=0; i<this.accumulation_tableData.length; i++ ){

        for( let j=0; j<this.accumulation_tableData.length; j++ ){
          if( this.accumulation_tableData[i].defSchemeType == this.accumulation_tableData[j].defSchemeType && i!=j ){
            ElMessage({
              message: '公积金名称重复！！',
              type: 'warning',
            })
            return
          }
        }

      }

      this.insertcompensation();

    },

    //新增参保方案
    insertcompensation(){
      this.axios({
        url: 'http://localhost:8010/provider/defInsured/insertDefInsured',
        method: 'post',
        data:{
          // 默认参保方案
          defInsured:{
            defInsuredName:this.schemeName
          },
          // 社保 方案
          defScheme1:this.social_tableData,
          // 公积金 方案
          defScheme2:this.accumulation_tableData,
          // 基数上限
          upper:this.security_cardinal_upper,
          // 基数下限
          lower:this.security_cardinal_lower,

          //取部门信息
          deptIds:this.$refs.tree.getCheckedKeys(),
          //取职位信息
          postIds:this.compensationForm.citysPost,
          //取员工信息
          staffIds:this.compensationForm.staffs
        }
      }).then(response => {
        console.log(response);
        if (response.data.data > 0) {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.selectEmps() // 修改完成后调用查询方法
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
        console.log(error);
      });

    },

    // 删除行
    deleteRow(index, rows) {
      rows.splice(index, 1);
      ElMessage({
        message: '删除成功',
        type: 'success',
      })
    },
    // 新增社保缴纳项目
    add_social() {
      let social_row = {
        defSchemeType: "养老保险", // 缴费项目
        defSchemeFloor: null, // 基数下限、
        defSchemeUpper: null, // 基数上限
        defSchemeFirmProp: null, // 公司缴纳比例
        defSchemePersonProp: null, // 个人缴纳比例
        defSchemeFirmSum: null, // 公司固定金额
        defSchemePersonSum: null, // 个人固定金额
      }
      this.social_tableData.push(social_row)
    },
    // 新增公积金缴纳项目
    add_accumulation() {
      let accumulation_row = {
        defSchemeType: "公积金", // 缴费项目
        defSchemeFloor: null, // 基数下限、
        defSchemeUpper: null, // 基数上限
        defSchemeFirmProp: null, // 公司缴纳比例
        defSchemePersonProp: null, // 个人缴纳比例
        defSchemeFirmSum: null, // 公司固定金额
        defSchemePersonSum: null, // 个人固定金额
      }
      this.accumulation_tableData.push(accumulation_row)
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

    // 查询员工
    selectStaff() {
      this.axios
          .get("http://localhost:8010/provider/staff/selectStaffXX")
          .then((response) => {
            console.log(response);
            this.staffData = response.data.data;
          })
          .catch(function (error) {
            console.log(error);
          });
    },

    // 选中员工的值
    // 多选删除按钮是否被禁用
    staffAll(val){
      // 选中的值
      this.tableVal=val
      // 清空选中 id
      this.compensationForm.staffs=[]

      // 循环获取选中行的id
      for(let i=0;i<val.length;i++){
        this.compensationForm.staffs.push( val[i].staffId )
      }
    },

    // 点击时 关闭 下拉框
    staffSelect(){
      // 关闭选择器
      this.$refs.vueSelect.blur();
    },

  },
  created() {
    this.selectPositionName();
    this.selectDeptName();
    this.selectStaff();
  },
  data() {
    // 格式
    const defaultProps = {
      children: 'children',
      label: 'deptName',
      value:'deptId'
    }
    return {
      // 多选时 数据
      tableVal:[],
      // // 选中时 员工id
      // staffIds:[],
      path: "/social/basic_setup/insured_scheme",
      // 方案名称
      schemeName: "",
      // 社保基数下限
      security_cardinal_lower: 0,
      // 社保基数上限
      security_cardinal_upper: 0,
      // 社保基数
      security_cardinal: 0,
      // 公积金基数
      accumulation_cardinal: 0,
      // 缴纳明细表 (显示/隐藏)
      payment_detail: false,
      // 参保组织
      compensationForm:{
        //部门
        dept:"",
        //存选中的职位
        citysPost:[],
        // 存放员工
        staffs:[],
      },
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
      //存储部门职位名称
      positionAll:[],

      // 员工弹出框
      become:false,
      // 员工数据
      staffData:[],


      // 社保缴纳项目
      social_options: [
        {
          value: '养老保险',
          label: '养老保险',
        },
        {
          value: '失业保险',
          label: '失业保险',
        },
        {
          value: '工伤保险',
          label: '工伤保险',
        },
        {
          value: '生育保险',
          label: '生育保险',
        },
        {
          value: '医疗保险',
          label: '医疗保险',
        },
      ],
      // 公积金缴纳项目
      accumulation_options: [
        {
          value: '公积金',
          label: '公积金',
        },
        {
          value: '补充公积金',
          label: '补充公积金',
        },
      ],
      // 社保缴纳表数据
      social_tableData: [
        {
          defSchemeType: "养老保险", // 缴费项目
          defSchemeFloor: null, // 基数下限
          defSchemeUpper: null, // 基数上限
          defSchemeFirmProp: null, // 公司缴纳比例
          defSchemePersonProp: null, // 个人缴纳比例
          defSchemeFirmSum: null, // 公司固定金额
          defSchemePersonSum: null, // 个人固定金额
        },
        {
          defSchemeType: "失业保险", // 缴费项目
          defSchemeFloor: null, // 基数下限、
          defSchemeUpper: null, // 基数上限
          defSchemeFirmProp: null, // 公司缴纳比例
          defSchemePersonProp: null, // 个人缴纳比例
          defSchemeFirmSum: null, // 公司固定金额
          defSchemePersonSum: null, // 个人固定金额
        },
        {
          defSchemeType: "工伤保险", // 缴费项目
          defSchemeFloor: null, // 基数下限、
          defSchemeUpper: null, // 基数上限
          defSchemeFirmProp: null, // 公司缴纳比例
          defSchemePersonProp: null, // 个人缴纳比例
          defSchemeFirmSum: null, // 公司固定金额
          defSchemePersonSum: null, // 个人固定金额
        },
        {
          defSchemeType: "生育保险", // 缴费项目
          defSchemeFloor: null, // 基数下限、
          defSchemeUpper: null, // 基数上限
          defSchemeFirmProp: null, // 公司缴纳比例
          defSchemePersonProp: null, // 个人缴纳比例
          defSchemeFirmSum: null, // 公司固定金额
          defSchemePersonSum: null, // 个人固定金额
        },
        {
          defSchemeType: "医疗保险", // 缴费项目
          defSchemeFloor: null, // 基数下限、
          defSchemeUpper: null, // 基数上限
          defSchemeFirmProp: null, // 公司缴纳比例
          defSchemePersonProp: null, // 个人缴纳比例
          defSchemeFirmSum: null, // 公司固定金额
          defSchemePersonSum: null, // 个人固定金额
        },
      ],
      // 公积金缴纳表数据
      accumulation_tableData: [
        {
          defSchemeType: "公积金", // 缴费项目
          defSchemeFloor: null, // 基数下限、
          defSchemeUpper: null, // 基数上限
          defSchemeFirmProp: null, // 公司缴纳比例
          defSchemePersonProp: null, // 个人缴纳比例
          defSchemeFirmSum: null, // 公司固定金额
          defSchemePersonSum: null, // 个人固定金额
        },
      ],
      //缴纳明细表数据
      payment_details_table: [
        {
          defSchemeType: "养老保险", // 缴纳项目
          cardinal_number: 999, // 基数
          defSchemeFirmProp: "11%", // 公司缴纳比例
          company_payment_money: 100, // 公司缴纳金额
          defSchemeFirmSum: 200, // 公司固定金额
          defSchemePersonProp: "6%", // 个人缴纳比例
          personage_payment_money: 66, // 个人缴纳金额
          defSchemePersonSum: 50, // 个人固定金额
        },
        {
          defSchemeType: "a保险", // 缴纳项目
          cardinal_number: 999, // 基数
          defSchemeFirmProp: "11%", // 公司缴纳比例
          company_payment_money: 100, // 公司缴纳金额
          defSchemeFirmSum: 200, // 公司固定金额
          defSchemePersonProp: "6%", // 个人缴纳比例
          personage_payment_money: 66, // 个人缴纳金额
          defSchemePersonSum: 50, // 个人固定金额
        },
        {
          defSchemeType: "b保险", // 缴纳项目
          cardinal_number: 999, // 基数
          defSchemeFirmProp: "11%", // 公司缴纳比例
          company_payment_money: 100, // 公司缴纳金额
          defSchemeFirmSum: 200, // 公司固定金额
          defSchemePersonProp: "6%", // 个人缴纳比例
          personage_payment_money: 66, // 个人缴纳金额
          defSchemePersonSum: 50, // 个人固定金额
        },
      ]
    };
  },
};
</script>

<style scoped>
.el-input {
  width: 50%;
  display: inline-block;
}

.all-input {
  /*  上、右、下、左  */
  padding: 25px 40px 25px 40px;
}

/*!* 内边距 *!*/
/*.j-card-body {*/
/*  padding: 1.5%;*/
/*}*/

.big-title {
  width: 100%;
  /*height: 45px;*/
  background-color: #f2f4fa;
  font-size: 16px;
  font-weight: bold;
  padding: 1%;
}

/* ::v-deep .el-input-number__decrease,::v-deep .el-input-number__increase {
  display: none;
}

::v-deep:hover .el-input-number__decrease,::v-deep:hover .el-input-number__increase {
  display: block;
} */

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

/* 外层div 内边距   */
.j-card-body {
  padding: 2% 4% 2% 2%;
}

@font-face {
  font-family: 'iconfont';  /* Project id 2990377 */
  src: url('//at.alicdn.com/t/font_2990377_1cwdx3n4atr.woff2?t=1639139822080') format('woff2'),
  url('//at.alicdn.com/t/font_2990377_1cwdx3n4atr.woff?t=1639139822080') format('woff'),
  url('//at.alicdn.com/t/font_2990377_1cwdx3n4atr.ttf?t=1639139822080') format('truetype');
}
.xxx{
  display: none;
}
</style>