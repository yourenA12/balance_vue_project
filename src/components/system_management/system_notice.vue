<!-- 公告页面 -->

<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body ">

        <div class="mt-20 ml-20 mr-20">
          <!-- 搜索数据部分 -->
          <el-form style="margin-bottom: -18px;" :inline="true" v-model="search">
            <!-- 公告标题搜索 -->
            <el-form-item class="form-gg" label="公告标题">
              <el-input size="small" placeholder="请输入公告标题" v-model="search.noticeTitle"></el-input>
            </el-form-item>
            <!-- 操作人员搜索 -->
            <el-form-item class="form-ry" label="发布人">
              <el-input size="small" placeholder="请输入发布人" v-model="search.noticePeople"></el-input>
            </el-form-item>
            <!-- 公告类型搜索 -->
            <el-form-item class="form-lx" label="类型">
              <el-select size="small" v-model="search.noticeType" placeholder="公告类型">
                <el-option
                    v-for="item in noticeTypes"
                    :key="item.value1"
                    :label="item.label"
                    :value="item.value1"
                ></el-option>

              </el-select>
            </el-form-item>

            <el-form-item class="search" style="margin-left: 10px;">
              <el-button @click="selectAllPage()" size="mini" class="search-ss" type="primary">
                <i class="iconfont">
                  &#xe61b
                </i>
                搜索
              </el-button>
              <el-button size="mini" class="search-cz" type="primary" @click="reset()">
                <i class="iconfont">
                  &#xe6b8
                </i>
                重置
              </el-button>
            </el-form-item>
          </el-form>

          <br />

          <!-- 操作按钮部分 -->
          <div class="button">
            <!-- 新增按钮 -->
            <el-button size="small" type="primary" plain @click="cancel(),outerVisible = true,judge='新增'" style="margin-left: 5px">
              <el-icon><i-plus/></el-icon>

                新增
            </el-button>

            <el-button size="small" type="danger" v-bind:disabled="disableds" plain @click="remove" style="margin-left: 10px;">
              <el-icon><i-delete /></el-icon>
              删除
            </el-button>

            <!-- 弹出操作窗口 -->
            <el-dialog width="670px"  v-model="outerVisible" destroy-on-close="false" >
              <span class="headline"> {{judge}}公告</span>
              <!-- form表单 -->
              <el-form class="announcement" v-model="affiche">
                <!-- 公告标题 -->
                <h2 class="headlines" >公告标题</h2>
                <el-form-item class="announcement-title">
                  <el-input size="small" placeholder="请输入公告标题" v-model="affiche.noticeTitle"></el-input>
                </el-form-item>
                <!-- 公告类型-->
                <h2 class="types" >公告类型</h2>
                <el-form-item class="announcement-type">
                  <el-select size="small" v-model="affiche.noticeType" placeholder="公告类型">
                    <el-option
                        v-for="item in noticeTypes"
                        :key="item.value1"
                        :label="item.label"
                        :value="item.value1"
                    ></el-option>
                  </el-select>
                </el-form-item>
                <!-- 公告状态 -->
                <h2 class="state">状态</h2>
                <el-form-item>
                  <div class="announcement-zt">
                    <el-radio v-model="affiche.noticeState" :label="0">正常</el-radio>
                    <el-radio v-model="affiche.noticeState" :label="1" style="margin-left: 16px">关闭</el-radio>
                  </div>
                </el-form-item>
                <!-- 选择部门 -->
                <h2 class="types">发布部门</h2>
                <el-select class="typexl" size="small" v-model="deptId" multiple ref="vueSelect" @change="onchange()" @click="onclicks()">
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
                </el-select><br/>

                <!-- 公告内容 -->
                <span class="neirong" >内容</span>
                <el-form-item>
                  <el-input
                      v-model="affiche.noticeMatter"
                      style="width: 514px;
                      font-weight: bold;
                      size: 14px;
                      margin-left: 77px;"
                      type="textarea">
                  </el-input>

                </el-form-item>

                <el-form-item >
                  <div class="announcement-cz">
                    <el-button size="mini" style="width: 65px;" @click="cancel()">
                      取消
                    </el-button>
                    <!--										&lt;!&ndash; 修改按钮 &ndash;&gt;"innerVisible = true"-->
                    <el-button size="mini" style="width: 65px;" type="primary" @click="judges()">
                      {{judge}}
                    </el-button>
                  </div>
                </el-form-item>
              </el-form>
            </el-dialog>


          </div>
        </div>



        <!-- 表格内容部分 -->
        <div class="sub-Content__primary">

          <el-table :data="tableData" style="width: 100%;margin-left: 24px;margin-bottom: 20px;"
                    @selection-change="deletepl"
                    :header-cell-style="{textAlign: 'center',background:'#F0F0F0',color:'#6C6C6C'}"
                    :cell-style="{ textAlign: 'center' }"
                    :default-sort="{ prop: 'date', order: 'descending' }"
                   >
            <!-- 全选操作按钮 -->
            <el-table-column type="selection" width="90" />

            <el-table-column  :index="indexMethod" type="index" label="序号" width="150" />
            <el-table-column prop="noticeTitle" label="公告标题" width="170" />
            <el-table-column prop="noticeType" label="公告类型" width="170" >
              <template #default="scope">
                {{ scope.row.noticeType==1?'公告':'通知' }}
              </template>
            </el-table-column>
            <el-table-column prop="noticeState" label="公告状态" width="160" >
              <template #default="scope">
                {{ scope.row.noticeState==0?'启用':'关闭' }}
              </template>
            </el-table-column>
            <el-table-column prop="noticePeople" label="发布人" width="160" />
            <el-table-column prop="updatedTime" label="发布时间" width="170" />
            <el-table-column align="center" label="操作" width="200">
              <template #default="scope">

                <el-button @click="checkRow(scope.row)" type="text" size="small">详情 </el-button>

                <el-button @click="updateRow(scope.row)" type="text" size="small">修改 </el-button>

                <el-popconfirm @confirm="deleteNotice(scope.row.noticeId,'one')"
                               title="确认要删除此公告吗?">
                  <template #reference>
                    <el-button type="text" size="small" style="color: orange">删除 </el-button>
                  </template>
                </el-popconfirm>
              </template>
            </el-table-column>
          </el-table>

        </div>
        <!-- 分页 -->
        <div class="demo-pagination-block">
          <!-- <span class="demonstration">All combined</span> -->
          <el-pagination
              v-model:currentPage="pageInfo.currentPage"
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

<!-- 详情弹出框 （抽屉） -->
  <el-drawer v-model="noticeCheck" :with-header="false">
    <h2 style="text-align: center">公告详情</h2>

    <br/><br/>

    <span style="font-size: 18px;font-weight: bold">公告标题：</span> <span> {{this.noticeDetail.noticeTitle}}</span><br/><br/>
    <span style="font-size: 18px;font-weight: bold">公告类型：</span> <span> {{this.noticeDetail.noticeType==0?'通知':'公告'}}</span><br/><br/>
    <span style="font-size: 18px;font-weight: bold">公告状态：</span> <span> {{this.noticeDetail.noticeState==0?'正常':'关闭'}}</span><br/><br/>
    <span style="font-size: 18px;font-weight: bold">发布人：</span> <span> {{this.noticeDetail.noticePeople}}</span><br/><br/>
    <span style="font-size: 18px;font-weight: bold">发布时间：</span> <span> {{this.noticeDetail.updatedTime}}</span><br/><br/>

    <div style="word-wrap:break-word; overflow:hidden">
      <span style="font-size: 18px;font-weight: bold">内容：</span>
      {{this.noticeDetail.noticeMatter}}</div><br/>

    <span style="font-size: 18px;font-weight: bold">发布部门：</span>
    <span v-for="item in noticeDept">{{item}}</span><br/><br/>

    <span style="font-size: 18px;font-weight: bold">已阅员工：</span>
    <span v-for="item in readStaff">{{item}}</span><br/><br/>

    <span style="font-size: 18px;font-weight: bold">未阅员工：</span>
    <span v-for="item in unreadStaff">{{item}}</span><br/><br/>

  </el-drawer>

</template>

<script>
import { defineComponent,ref} from 'vue'
import { ElMessageBox, ElMessage } from 'element-plus'

import { ElNotification } from 'element-plus'

export default {
  data() {
    // 格式
    const defaultProps = {
      children: 'children',
      label: 'deptName',
      value:'deptId'
    }
    //批量删除提示框
    const remove = () => {
      ElMessageBox.confirm(
          '确定删除！！！',
          {
            confirmButtonText: '确定',
            cancelButtonText: '取消'
          }
      )
          .then(() => {

            // 批量删除方法
            this.deleteNotice(this.deleteNoticeIds,'all')

          })
          .catch(() => {
            ElMessage({
              message: '取消',
            })
          })
    }

    //表格批量删除
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



      //表格批量删除
      remove,
      // 删除 公告ids
      deleteNoticeIds:[],
      // 某条公告详情
      noticeDetail:null,
      // 公告发布部门
      noticeDept:[],
      // 公告已阅员工
      readStaff:[],
      // 未读员工
      unreadStaff:[],

      // 公告 抽屉
      noticeCheck:false,


      //搜索重置form
      search:{
        noticeTitle: '', // 标题
        noticePeople:'', //发布人
        noticeType: '', // 类型
      },

      // 分页
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
      },

      // 表格数据
      tableData: [],

      // 弹出框数据
      affiche:{
        noticeTitle: '', // 标题
        noticeType: '', // 类型
        noticeState: '', // 状态
        deptIds:[], // 部门s
        noticeMatter:'',// 内容
      },

      // 公告发布部门下拉框数据
      deptIds:[],

      // 公告类型下拉框数据
      noticeTypes:[
        {
          value1: 0,
          label: '通知',
        },
        {
          value1: 1,
          label: '公告',
        }
      ],
      //显示添加还是有修改的状态
      outerVisible: ref(false),

      // 判断添加还是编辑
      judge: "添加",

      //按钮是否被禁用
      disableds:true,

      // 多选时 数据
      tableVal:[],
    }
  },
  methods:{


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


    // 新增公告方法
    insertNotice(){

      this.axios({
        url: 'http://localhost:8010/provider/notice/insertOneNotice',
        method: 'post',
        data:{
          // 发布人员工
          staffId:this.$store.state.userMsg.staffId,
          // 发布人姓名
          noticePeople:this.$store.state.userMsg.staffName,
          //公告标题
          noticeTitle:this.affiche.noticeTitle,
          //公告类型
          noticeType:this.affiche.noticeType,
          //公告状态
          noticeState:this.affiche.noticeState,
          //内容
          noticeMatter:this.affiche.noticeMatter,
          //部门
          deptIds:this.res2,

        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.outerVisible = false
          this.selectAllPage() // 修改完成后调用查询方法
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
        console.log(error);
      });

    },

    /*序号*/
    indexMethod(index) {
      let curpage = this.pageInfo.currentPage; //单前页码，具体看组件取值
      let limitpage = this.pageInfo.pagesize; //每页条数，具体是组件取值
      // return index + 1 + (curpage - 1) * limitpage;

      return this.pageInfo.total - (index + (curpage - 1) * limitpage);
    },

    // 查询所有公告
    selectAllPage() {

      this.axios
          .get("http://localhost:8010/provider/notice/selectAllPage",
            {
              params: {
                currentPage:this.pageInfo.currentPage,
                pagesize:this.pageInfo.pagesize,
                noticeTitle:this.search.noticeTitle,
                noticePeople:this.search.noticePeople,
                noticeType:this.search.noticeType
              }
            })
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

    // 查询所有部门
    selectAllDept() {
      this.axios
          .get("http://localhost:8010/provider/notice/selectAllDept")
          .then((response) => {
            console.log(response);
            this.deptIds=response.data.data
          })
          .catch(function (error) {
            console.log(error);
          });
    },

    // 按公告id查询所有部门
    selectAllDeptByNoticeId(noticeId) {
      this.axios
          .get("http://localhost:8010/provider/notice/selectAllDeptByNoticeId/"+ noticeId)
          .then((response) => {
            console.log(response);
            for (let i=0;i<response.data.data.length;i++){
              this.affiche.deptIds.push(response.data.data[i].deptId)
              this.noticeDept.push(response.data.data[i].deptName + "、 " )
            }

            this.$nextTick(() => {
              // 将值赋值到选择器中
              this.$refs.tree.setCheckedKeys(this.affiche.deptIds, false)
            })

          })
          .catch(function (error) {
            console.log(error);
          });
    },

    // 向后台传输多个id 进行删除
    deleteNotice(noticeIds,type) {

      if(type=='one'){
        this.deleteNoticeIds=[]
        // 添加
        this.deleteNoticeIds.push(noticeIds)
      }

      this.axios
          .delete("http://localhost:8010/provider/notice/deleteNotices/" +  this.deleteNoticeIds)
          .then((response) => {
            console.log(response);
              if(response.data.data>0){
                ElMessage({
                  type: 'success',
                  message: '删除成功！！',
                })
                // 调用查询
                this.selectAllPage()
              }else{
                ElMessage('删除失败！！')
              }
          })
          .catch(function (error) {
            console.log(error);
          });
    },

    // 点击一行公告查看详情
    checkRow(row){
      //查询当前公告
      this.noticeDetail=row
      // 打开抽屉
      this.noticeCheck=true
      // 清空部门
      this.affiche.deptIds=[]
      this.noticeDept=[]
      // 按公告id查询发布部门
      this.selectAllDeptByNoticeId(row.noticeId)
      //清空员工
      this.readStaff=[]
      this.unreadStaff=[]
      // 按公告id查询员工
      this.selectNoticeStaffByNoticeId(row.noticeId,row.staffId)

      alert( row.noticeId );
      alert( row.staffId )

    },

    // 查询公告员工表数据
    selectNoticeStaffByNoticeId(noticeId,staffId) {
      this.axios
          .get("http://localhost:8010/provider/notice/selectNoticeStaffByNoticeId/"+ noticeId+"/"+staffId)
          .then((response) => {
            console.log(response);
            for (let i=0;i<response.data.data.length;i++){

              if( response.data.data[i].noticeState==1 ){
                this.readStaff.push(response.data.data[i].staffName + "、 ")
              }else{
                this.unreadStaff.push(response.data.data[i].staffName + "、 " )
              }

            }

          })
          .catch(function (error) {
            console.log(error);
          });
    },

    // 点击修改按钮先传值
    updateRow(row){
      // 打开弹出框
      this.outerVisible=true
      // 状态改为修改
      this.judge='修改'
      // 一行的数据
      this.affiche=row
      // 清空部门
      this.affiche.deptIds=[]
      // 按公告id查询发布部门
      this.selectAllDeptByNoticeId(row.noticeId)

    },

    // 修改单条部门数据
    updateNotice() {

      console.error( this.affiche.deptIds )

      this.affiche.deptIds=this.res2

      this.axios
          .put("http://localhost:8010/provider/notice/updateOneNotice" ,this.affiche)
          .then((response) => {
            console.log(response);
            if(response.data.data>0){
              ElMessage({
                type: 'success',
                message: '修改成功！！',
              })
              // 关闭弹出框
              this.outerVisible=false
              // 调用查询
              this.selectAllPage()
            }else{
              ElMessage('修改失败！！')
            }
          })
          .catch(function (error) {
            console.log(error);
          });
    },


    // 多选删除按钮是否被禁用
    deletepl(val){
      // 选中的值
      this.tableVal=val
      // 清空选中 id
      this.deleteNoticeIds=[]

      // 循环获取选中行的id
      for(let i=0;i<val.length;i++){
        this.deleteNoticeIds.push( val[i].noticeId )
      }

      // 如果没有选中 将删除按钮禁用
      if(this.tableVal != ''){
        this.disableds=false
      }else {
        this.disableds=true
      }
    },


    //新增或修改方法判断方法
    judges(){
      if(this.judge==="新增"){
        this.insertNotice();
      }else{
        this.updateNotice()
      }
    },
    //取消按钮方法
    cancel(){
      this.affiche={}
      this.$nextTick(() => {
        // 将值赋值到选择器中
        this.$refs.tree.setCheckedKeys([], false)
      })
      this.outerVisible = false
      this.selectAllPage()
    },
    //重置
    reset(){
      //搜索重置form
      this.search={
        noticeTitle: '', // 标题
        noticePeople:'', //发布人
        noticeType: '', // 类型
      }
      // 调用查询
      this.selectAllPage()
    }
  },
  created() {
    // 分页
    this.selectAllPage()
    // 查询所有部门
    this.selectAllDept()

    this.selectDeptName()
  }
}
</script>

<style type="text/css" scoped>
@import url("../../css/zpdaohang.css");
/* 图标 */
@font-face {
  font-family: 'iconfont';
  /* Project id 2994452 */
  src: url('//at.alicdn.com/t/font_2994452_60uvtx3m6is.woff2?t=1638864192788') format('woff2'),
  url('//at.alicdn.com/t/font_2994452_60uvtx3m6is.woff?t=1638864192788') format('woff'),
  url('//at.alicdn.com/t/font_2994452_60uvtx3m6is.ttf?t=1638864192788') format('truetype');
}

/*内容*/
  .neirong{
    font-weight: bold;
    size: 14px;
    margin-left: 37px;
    position: absolute;
    top: 192px;
  }
/* 分页 */
.demo-pagination-block{
  margin-left: 617px;
  margin-bottom: 20px;
}
.headline{
  font-size: 18px;
  font-weight:bold;
  position: absolute;
  top: 16px;
}
.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}
.tck{
  width: 600px;
  height: 200px;
  background: green;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

/* 对话公告框标题文本样式 */
.announcement .headlines{
  font-size: 14px;
  margin-left: 12px;
  margin-top: 20px;
}
/* 对话框公告类型下拉框样式 */
.announcement .types{
  font-size: 14px;
  margin-left: 360px;
  margin-top:-51px;
}
/* 对话框公告状态单选框样式 */
.announcement .state{
  font-size: 14px;
  margin-left: 37px;
  margin-top:19px;
}
/* 对话框公告内容文本框样式 */
.announcement .content{
  font-size: 14px;
  margin-left: 37px;
  margin-top:23px;
}

/* 弹出窗口按钮样式 */
.announcement .announcement-cz{
  width: 150px;
  margin-left: 451px;
  margin-bottom: -15px;
}
/* 弹出窗口内容样式 */
.announcement .announcement-nr {
  width: 525px;
  margin-top: -20px;
  margin-left: 80px;
  margin-bottom: -45px;

}
/* 弹出窗口状态样式 */
.announcement .announcement-zt {
  margin-top: -30px;
  margin-left: 80px;
}
/* 弹出窗口类型样式 */
.announcement .announcement-type {
  width: 240px;
  margin-top: -30px;
  margin-left: 428px;

}
/* 弹出窗口标题样式 */
.announcement .announcement-title {
  width: 240px;
  margin-top: -30px;
  margin-left: 80px;

}
.button{
  margin-bottom: 10px;

}
/* 表头新增按钮 */
.button .button-new {
  height: 30px;
  width: 90px;
  color: #1890ff;
  background: #e8f4ff;
  border-color: #a3d3ff;
  margin: 0px 0px 0px 10px;
  border: none;
  width: 106px;
}

.button .button-new:hover {
  height: 30px;
  width: 90px;
  color: white;
  background-color: #1890FF;
  margin: 0px 0px 0px 10px;
  border: none;
  width: 106px;
}

/* 表头批量删除按钮样式 */
.button-delete {
  height: 30px;
  width: 90px;
  margin: 0px 0px 0px 5px;
  border: none;
  width: 106px;
}

/* 搜索 */
.form-gg {
  margin: 0px 0px 0px 10px;
}

.form-lx {
  margin: 0px 0px 0px 10px;
}

.form-ry {
  margin: 0px 0px 0px 10px;
}


/* 搜索按钮 */
.search-ss {
  background-color: #085FC3;
  color: white;
  margin: 29px 0px 0px 5px;
  width: 85px;
  color: #fff;
  background-color: #1890ff;
  border-color: #1890ff;
}
.search-ss:hover {
  background-color: #085FC3;
  color: white;
  margin: 29px 0px 0px 5px;
  width: 85px;
  color: #1890ff;
  background: #e8f4ff;
  border-color: #a3d3ff;
}
/* 重置按钮 */
.search-cz {
  color: black;
  margin: 29px 0px 0px 5px;
  width: 85px;
  background: #fff;
  border: 1px solid #dcdfe6;
  border-top-color: rgb(220, 223, 230);
  border-right-color: rgb(220, 223, 230);
  border-bottom-color: rgb(220, 223, 230);
  border-left-color: rgb(220, 223, 230);
  border-color: #dcdfe6;
  color: #606266;
}
.search-cz:hover {

  color: black;
  margin: 29px 0px 0px 5px;
  width: 85px;
  background: #fff;
  border: 1px solid #dcdfe6;
  color: #1890ff;
  background: #e8f4ff;
  border-color: #a3d3ff;
}


.typexl{
  position: absolute;
  left: 448px;
  top:130px

}

.xxx{
  display: none;
}


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


.mr-20 {
  margin-right: 20px;
}

.ml-20 {
  margin-left: 20px;
}

.mt-20 {
  margin-top: 0px;
}

a {
  color: #085fc3;
  background-color: transparent;
}

a, area, button, [role="button"], input:not([type="range"]), label, select, summary, textarea {
  touch-action: manipulation;
}

a {
  color: #366cb3;
  text-decoration: none;
  background-color: transparent;
  outline: none;
  cursor: pointer;
  transition: color 0.3s;
  -webkit-text-decoration-skip: objects;
}

button::-moz-focus-inner, [type="button"]::-moz-focus-inner, [type="reset"]::-moz-focus-inner, [type="submit"]::-moz-focus-inner {
  border-style: none;
}

button::-moz-focus-inner, [type="button"]::-moz-focus-inner, [type="reset"]::-moz-focus-inner, [type="submit"]::-moz-focus-inner {
  padding: 0;
  border-style: none;
}

.ant-btn::before {
  background: #fff;
  border-radius: inherit;
}

.ant-btn::before {
  position: absolute;
  top: -1px;
  right: -1px;
  bottom: -1px;
  left: -1px;
  z-index: 1;
  display: none;
  background: #fff;
  border-radius: inherit;
  opacity: 0.35;
  transition: opacity 0.2s;
  content: '';
  pointer-events: none;
}

button, html [type="button"], [type="reset"], [type="submit"] {
  -webkit-appearance: button;
}

.ant-btn-primary {
  color: #fff;
  background-color: #085fc3;
  border-color: #085fc3;
  box-shadow: 0 2px 0 rgba(0, 0, 0, 0.045);
}

.ant-btn-primary {
  color: #fff;
  background-color: #366cb3;
  border-color: #366cb3;
  text-shadow: 0 -1px 0 rgba(0, 0, 0, 0.12);
  box-shadow: 0 2px 0 rgba(0, 0, 0, 0.045);
}

.ant-btn, .ant-btn:active, .ant-btn:focus {
  outline: 0;
}

.ant-btn {
  line-height: 1.499;
  position: relative;
  display: inline-block;
  font-weight: 400;
  white-space: nowrap;
  text-align: center;
  background-image: none;
  border: 1px solid transparent;
  box-shadow: 0 2px 0 rgba(0, 0, 0, 0.015);
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
  user-select: none;
  touch-action: manipulation;
  height: 32px;
  padding: 0 15px;
  font-size: 14px;
  border-radius: 3px;
  color: #606c82;
  background-color: #fff;
  border-color: #d3dae2;
}

.ant-btn-primary {
  color: #fff;
  background-color: #085fc3;
  border-color: #085fc3;
  box-shadow: 0 2px 0 rgba(0, 0, 0, 0.045);
}

.demo-pagination-block{
  margin-left:850px ;
  margin-top: 20px;
  margin-bottom: 30px;
}




</style>
