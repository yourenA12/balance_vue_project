<template>
  <div id="background">
    <div class="container">
      <br>
      <h1 style="color:#707070;font-family: inherit;
    font-weight: 500;
    line-height: 1.1;">Login</h1>
      <div class="form">
        <div class="item">
<!--          <label style="color:#707070;">手机号:</label>-->
          <el-input type="text"  v-model="rsFrom.staffPhone"  placeholder="请输入用户名" style="width: 323px;">
            <template #prefix>
            <el-icon style="margin-top:13px"><i-user-filled /></el-icon>
            </template>
          </el-input>
          <br/>
        </div>
        <div class="item">
          <el-input
              v-model="rsFrom.staffPass"
              type="password"
              placeholder="请输入密码"
              show-password
              style="width: 323px">
            <template #prefix>
              <el-icon style="margin-top:13px"><i-unlock /></el-icon>
            </template>
          </el-input>
          <br/>
        </div>
      </div>
      <div class="e9login-element" style="padding-top: 0px;top: 238px;left: 80px;width: 324px;height: 45px;position: absolute;">
        <div class="e9login-form-submit e9login-submit" style="width: 324px; height: 45px;">
          <el-button @click="toLogin()" :loading="loading"  style="width: 323px; height: 25px;background:#1890ff;  font-size: 18px; border-radius: 6px;margin-left: -80px " type="button" >
            <span style="font-size: 14px;color: white">登 录</span>
          </el-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {ElMessage } from 'element-plus'
// import { Calendar, Unlock } from '@element-plus/icons-vue'
import { ref } from 'vue'
const input = ref('')
export default {
  data(){
    return{
      loading:false,
      rsFrom:{
        staffPhone: '',
        staffPass: ''
      },
      //staffName:''
    }
  },
  methods:{
    toLogin() {

      if(this.rsFrom.staffPhone=="" || this.rsFrom.staffPass==""){
        ElMessage({
          type:'warning',
          message:'请输入手机号'
        })
        return
      }else if(this.rsFrom.staffPass==""){
        ElMessage({
          type:'warning',
          message:'请输入密码'
        })
        return
      }


      this.axios({
        method:'post',
        url:"http://localhost:8010/provider/user/toLogin",
        data:this.rsFrom,
        responseType:'json',
        responseEncoding:'utf-8',
      }).then(response=>{
        console.log(response)
        if(response.data.data !== null){
          ElMessage({
            type:'success',
            message:'登录成功'
          })
          //this.staffName===response.data.data.staffName
          this.$store.state.userMsg = response.data.data

          console.error(response.data.data)
          console.error(this.$store.state.userMsg)

          this.$router.push({path:'/home'})
        }else{
          ElMessage.error("手机号或密码错误！！")
        }
      })
    }
  }
}
</script>


<style scoped>
.ant-btn:hover{
  color: #000;
}
/*!*按钮悬浮*!*/
/*.ant-btn:hover {*/
/*  background: #126c9e !important;*/
/*  color: white !important;*/
/*  border-color: #01a8f9 !important;*/
/*}*/

#background{
  width: 100%;
  height: 100%;
  background: url("../assets/1234567.jpg");
  background-size:100% 100%;
  position: fixed;
  top: 0;
  left: 0;
}
.container{
  width: 400px;
  height: 330px;
  position: absolute;
  top: 40%;
  left: 50%;
  transform: translate(-50%,-50%);
  background:white;
  text-align: center;
  border-radius: 7px;
  /*filter:alpha(opacity=60);*/
  /*-moz-opacity:0.6;*/
  /*opacity:0.60;*/
}
.container h1{
  color: aliceblue;
  margin-left: 20px;
}
.item {
  color: white;
  margin-left: 10%;
  margin-top: 35px;
  font-size: 20px;
  text-align: left;
}
.item label{
  float:left;
  width: 5em;
  margin-right: 1em;
  text-align: right;
}

.keep{
  color: white;
}
.keep input{
  width: 15px;
  height: 15px;
  margin-top: 7px;
  margin-left: 10px;
  margin-right: 10px;
}

</style>
