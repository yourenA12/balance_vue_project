<!--员工编辑个人信息页面-->
<template>
  <div class="basicfile_big">
    <div class="basicfile_big_auto">

<!--      工作经验-->
      <div style="border-left:3px solid blue;">
        <h3>
          <span style="margin-left:10px;">工作经验</span>
        </h3>
      </div>

<!--      工作经历-->
      <div class="information">
        <h3 style="color: #085fc3;font-size: 14px;margin-left: 10px;display: inline-block;">工作经历<i class="iconfont" style="color: #085fc3;margin-right:2px;">&#xe604;</i></h3>
        <div style="width:840px;border-top:1px solid silver;display: inline-block;margin-left: 7px;margin-bottom: 5px;"></div>
        <h3 v-show="workerButton" style="color: #085fc3;font-size: 14px;position: relative;margin-left: 10px;display: inline-block;"><el-button type="text" @click="addButton()"><i class="iconfont" style="color: #085fc3;margin-right:2px;">&#xe613;</i>添加</el-button></h3>
      </div>

      <div style="width: 100%;margin-top: 20px;">

        <div style="width:98%;height:80px;border: 1px dotted silver;margin: auto" v-show="addWork">
          <div style="margin: auto;width:120px;height: 80px;line-height: 80px">
            <el-button type="text" @click="addWorks()"><i class="iconfont" style="color: #085fc3;margin-right:2px;">&#xe613;</i>添加&nbsp;&nbsp;工作经历</el-button>
          </div>
        </div>

        <div v-for="(obj,index) in workAll" style="width:100%;position: relative;margin-top: 20px;" v-show="workShow">
          <div class="information_text">
            <ul style="list-style-type: none; ">
              <li>
                <label>开始时间</label>
                <p>{{obj.workStareTime}}</p>
              </li>
              <li>
                <label>结束时间</label>
                <p>{{obj.workEndTime}}</p>
              </li>
              <li>
                <label>任职公司</label>
                <p>{{obj.companyName}}</p>
              </li>
              <li>
                <label>职位</label>
                <p>{{obj.positionName}}</p>
              </li>
            </ul>
          </div>
          <div style="position: absolute;right: 6px;top:-5px">
          <el-button type="text" style="color: #085fc3;" @click="workEditor(index)">编辑 </el-button>
          <el-button type="text" style="color: red;" @click="deleteWorkId(obj)">删除 </el-button>
          </div>
          <br/>
        </div>

      <!--添加工作经历-->
        <div class="information_from" v-show="workInput">
          <el-form style="width: 90%;margin: auto; " :rules="rules" ref="workForm" :model="workForm">
             <br/>
              <div style="display: inline-block;margin:20px 0px 0px 50px">
                <el-form-item label="开始时间:" required >
                  <el-col :span="11" >
                    <el-form-item prop="workStareTime" style="width:240px;">
                        <el-date-picker type="date" placeholder="选择日期" v-model="workForm.workStareTime"  style="width: 100%;"></el-date-picker>
                    </el-form-item>
                  </el-col>
                </el-form-item><br>

                <el-form-item label="任职公司：" class="el-form-item" >
                  <el-col :span="11">
                    <el-form-item prop="companyName" style="width:240px;">
                      <el-input v-model="workForm.companyName"   style="width: 100%;"></el-input>
                    </el-form-item>
                  </el-col>
                </el-form-item><br>

              </div>

            <div style="display: inline-block;position: absolute;top:20px;right:150px">
              <br>
                <el-form-item label="结束时间:" required >
                  <el-col :span="11">
                    <el-form-item prop="workEndTime" style="width:240px;">
                      <el-date-picker type="date" placeholder="选择日期" style="width: 100%;" v-model="workForm.workEndTime"></el-date-picker>
                    </el-form-item>
                  </el-col>
                </el-form-item><br>


                <div style="float: right">
                <el-form-item label="职位：" >

                    <el-form-item prop="positionName" style="width:240px;">
                      <el-input v-model="workForm.positionName" style="width: 100%;"></el-input>
                    </el-form-item>

                </el-form-item>
                </div>
            </div>

            <div style="width:90%;height:60px;margin: auto;margin-top:30px;padding: 0px 0px 30px 0px">
              <div style="width:20%;height:50px;margin:auto;">
                <el-button @click="workCancel()">取消</el-button>
                <el-button type="primary" @click="insertWork()">保存</el-button>
              </div>
            </div>
          </el-form>
        </div>

    </div>

<!--      奖罚记录-->
      <div style="border-left:3px solid blue;margin-top:20px;">
        <h3>
          <span style="margin-left:10px;">奖罚记录</span>
        </h3>
      </div>
      <div class="information">
        <h3 style="color: #085fc3;font-size: 14px;margin-left: 10px;display: inline-block;">荣誉与奖励<i class="iconfont" style="color: #085fc3;margin-right:2px;">&#xe604;</i></h3>
        <div style="width:830px;border-top:1px solid silver;display: inline-block;margin-left: 7px;margin-bottom: 5px;"></div>
        <h3 v-show="honorsButton" style="color: #085fc3;font-size: 14px;position: relative;margin-left: 5px;display: inline-block;"><el-button type="text" @click="addhonorsButton()"><i class="iconfont" style="color: #085fc3;margin-right:2px;">&#xe613;</i>添加</el-button></h3>
      </div>

      <div style="width: 100%;margin-top: 20px;">

        <div style="width:98%;height:80px;border: 1px dotted silver;margin: auto" v-show="addHonors">
          <div style="margin: auto;width:120px;height: 80px;line-height: 80px">
            <el-button type="text" @click="addHonorss()"><i class="iconfont" style="color: #085fc3;margin-right:2px;">&#xe613;</i>添加&nbsp;&nbsp; 荣誉与奖励</el-button>
          </div>
        </div>

        <div v-for="(obj,index) in honorsAll" style="width:100%;position: relative;margin-top: 20px;" v-show="honorsShow">
          <div class="information_text">
            <ul style="list-style-type: none; ">

              <li>
                <label>荣誉/奖项名称</label>
                <p>{{obj.gloryName}}</p>
              </li>
              <li>
                <label>奖励日期</label>
                <p>{{obj.createdTime}}</p>
              </li>
              <li>
                <label>颁发单位名称</label>
                <p>{{obj.gloryUnitname}}</p>
              </li>
              <li>
                <label>备注</label>
                <p>{{obj.gloryRemark}}</p>
              </li>

            </ul>
          </div>
          <div style="position: absolute;right: 6px;top:-5px">
            <el-button type="text" style="color: #085fc3;" @click="honorsEditor(index)">编辑 </el-button>
            <el-button type="text" style="color: red;" @click="deleteGloryId(obj)">删除 </el-button>
          </div><br/>
        </div>

        <div class="information_from" v-show="honorsInput">
          <el-form style="width: 90%;margin: auto; " :rules="rules" ref="honorsForm" :model="honorsForm">
            <br/>
            <div style="display: inline-block;margin:20px 0px 0px 50px">
              <el-form-item label="荣誉/奖项名称:" prop="gloryName" >
              <el-input v-model="honorsForm.gloryName" style="width:240px;"></el-input>
            </el-form-item><br>

              <el-form-item label="颁发单位名称：" class="el-form-item" >
                <el-col :span="11">
                  <el-form-item prop="gloryUnitname" style="width:240px;">
                    <el-input   v-model="honorsForm.gloryUnitname" style="width: 100%;"></el-input>
                  </el-form-item>
                </el-col>
              </el-form-item><br>

            </div>

            <div style="display: inline-block;position: absolute;top:20px;right:150px">
              <br>
              <el-form-item label="奖励日期:" >
                <el-col :span="11">
                  <el-form-item prop="createdTime" style="width:240px;">
                    <el-date-picker type="date" placeholder="选择日期" v-model="honorsForm.createdTime" style="width: 100%;"></el-date-picker>
                  </el-form-item>
                </el-col>
              </el-form-item><br>


              <div style="float: right">
                <el-form-item label="备注：" >

                  <el-form-item prop="gloryRemark" style="width:240px;">
                    <el-input type="textarea"  v-model="honorsForm.gloryRemark" style="width: 100%;"></el-input>
                  </el-form-item>

                </el-form-item>
              </div>
            </div>


            <div style="width:90%;height:60px;margin: auto;margin-top:30px;padding: 0px 0px 30px 0px">
              <div style="width:20%;height:50px;margin:auto;">
                <el-button @click="honorsCancel()">取消</el-button>
                <el-button type="primary" @click="insertGlory()">保存</el-button>
              </div>
            </div>
          </el-form>
        </div>

      </div>

      <!--      惩罚-->
      <div class="information">
        <h3 style="color: #085fc3;font-size: 14px;margin-left: 10px;display: inline-block;">惩罚<i class="iconfont" style="color: #085fc3;margin-right:2px;">&#xe604;</i></h3>
        <div style="width:870px;border-top:1px solid silver;display: inline-block;margin-left: 7px;margin-bottom: 5px;"></div>
        <h3 v-show="punishmentButton" style="color: #085fc3;font-size: 14px;position: relative;margin-left: 5px;display: inline-block;"><el-button type="text" @click="addpunishmentButton()"><i class="iconfont" style="color: #085fc3;margin-right:2px;">&#xe613;</i>添加</el-button></h3>
      </div>

      <div style="width: 100%;margin-top: 20px;">

        <div  style="width:98%;height:80px;border: 1px dotted silver;margin: auto" v-show="addPunishment">
          <div style="margin: auto;width:120px;height: 80px;line-height: 80px">
            <el-button type="text" @click="addPunishments()"><i class="iconfont" style="color: #085fc3;margin-right:2px;">&#xe613;</i>添加&nbsp;&nbsp; 惩罚</el-button>
          </div>
        </div>

        <div v-for="(obj,index) in punishmentAll" style="width:100%;position: relative;margin-top: 20px;" v-show="punishmentShow">
          <div class="information_text">
            <ul style="list-style-type: none; ">

              <li>
                <label>惩罚类型</label>
                <p>{{obj.punishType}}</p>
              </li>
              <li>
                <label>惩罚原因</label>
                <p>{{obj.punishCause}}</p>
              </li>
              <li>
                <label>惩罚单位</label>
                <p>{{obj.punishUnit}}</p>
              </li>
              <li>
                <label>是否撤销</label>
                <p>{{obj.isRevocation==0?'否':'是'}}</p>
              </li>
              <li>
                <label>备注</label>
                <p>{{obj.punishRemark}}</p>
              </li>
            </ul>
          </div>
          <div style="position: absolute;right: 6px;top:-5px">
            <el-button type="text" style="color: #085fc3;" @click="punishmentEditor(index)">编辑 </el-button>
            <el-button type="text" style="color: red;" @click="deletePunishId(obj)">删除 </el-button>
          </div><br/>
        </div>

        <div class="information_from" v-show="punishmentInput">
          <el-form style="width: 90%;margin: auto; " :rules="rules" ref="punishmentForm" :model="punishmentForm">
            <br/>
            <div style="display: inline-block;margin:20px 0px 0px 50px">
              <el-form-item label="惩罚类型:" prop="punishType">
                <el-select v-model="punishmentForm.punishType" placeholder="请选择惩罚类型" style="width: 240px;">
                  <el-option label="警告" value="警告"></el-option>
                  <el-option label="记过" value="记过"></el-option>
                  <el-option label="记大过" value="记大过"></el-option>
                  <el-option label="降职" value="降职"></el-option>
                  <el-option label="辞退" value="辞退"></el-option>
                </el-select>
              </el-form-item><br>

              <el-form-item label="惩罚单位：" class="el-form-item" >
                <el-col :span="11">
                  <el-form-item prop="punishUnit" style="width:240px;">
                    <el-input   v-model="punishmentForm.punishUnit" style="width: 100%;"></el-input>
                  </el-form-item>
                </el-col>
              </el-form-item><br>

              <el-form-item label="惩罚备注：" >
                <el-form-item prop="punishRemark" style="width:240px;">
                  <el-input type="textarea"  v-model="punishmentForm.punishRemark" style="width: 100%;"></el-input>
                </el-form-item>

              </el-form-item>

            </div>

            <div style="display: inline-block;position: absolute;top:20px;right:150px">
              <br>
              <div style="float: right">

                <el-form-item label="惩罚原因：" >
                  <el-form-item prop="punishCause" style="width:240px;">
                    <el-input   v-model="punishmentForm.punishCause" style="width: 100%;"></el-input>
                  </el-form-item>

                </el-form-item>

                <el-form-item label="是否撤销：" >
                  <el-radio-group v-model="punishmentForm.isRevocation" style="position: absolute;left:2px;">
                    <el-radio :label="0">否</el-radio>
                    <el-radio :label="1">是</el-radio>
                  </el-radio-group>
                </el-form-item>
              </div>
            </div>


            <div style="width:90%;height:60px;margin: auto;margin-top:30px;padding: 0px 0px 30px 0px">
              <div style="width:20%;height:50px;margin:auto;">
                <el-button @click="punishmentCancel()">取消</el-button>
                <el-button type="primary" @click="insertPunish()">保存</el-button>
              </div>
            </div>
          </el-form>
        </div>

      </div>


      <!--      教育经历-->
      <div style="border-left:3px solid blue;margin-top: 20px;">
        <h3>
          <span style="margin-left:10px;">教育经历</span>
        </h3>
      </div>

      <!--      教育经历-->
      <div class="information">
        <h3 style="color: #085fc3;font-size: 14px;margin-left: 10px;display: inline-block;">教育经历<i class="iconfont" style="color: #085fc3;margin-right:2px;">&#xe604;</i></h3>
        <div style="width:840px;border-top:1px solid silver;display: inline-block;margin-left: 7px;margin-bottom: 5px;"></div>
        <h3 v-show="educationalButton" style="color: #085fc3;font-size: 14px;position: relative;margin-left: 10px;display: inline-block;"><el-button type="text" @click="addeducationalButton()"><i class="iconfont" style="color: #085fc3;margin-right:2px;">&#xe613;</i>添加</el-button></h3>
      </div>

      <div style="width: 100%;margin-top: 20px;">

        <div style="width:98%;height:80px;border: 1px dotted silver;margin: auto" v-show="addEducational">
          <div style="margin: auto;width:120px;height: 80px;line-height: 80px">
            <el-button type="text" @click="addeducationals()"><i class="iconfont" style="color: #085fc3;margin-right:2px;">&#xe613;</i>添加&nbsp;&nbsp;教育经历</el-button>
          </div>
        </div>

        <div v-for="(obj,index) in educationalAll" style="width:100%;position: relative;margin-top: 20px;" v-show="educationalShow">
          <div class="information_text">
            <ul style="list-style-type: none; ">
              <li>
                <label>开始时间</label>
                <p>{{obj. educationStartTime}}</p>
              </li>
              <li>
                <label>结束时间</label>
                <p>{{obj.educationEndTime}}</p>
              </li>
              <li>
                <label>学校名称</label>
                <p>{{obj.educationStudentname}}</p>
              </li>
              <li>
                <label>所属专业</label>
                <p>{{obj.educationMajor}}</p>
              </li>
            </ul>
          </div>
          <div style="position: absolute;right: 6px;top:-5px">
            <el-button type="text" style="color: #085fc3;" @click="educationalEditor(index)">编辑 </el-button>
            <el-button type="text" style="color: red;" @click="deleteEducationId(obj)">删除 </el-button>
          </div>
        </div><br/>

        <div class="information_from" v-show="educationalInput">
          <el-form style="width: 90%;margin: auto; " :rules="rules" ref="educationalForm" :model="educationalForm">
            <br/>
            <div style="display: inline-block;margin:20px 0px 0px 50px">
              <el-form-item label="开始时间:" required >
                <el-col :span="11" >
                  <el-form-item prop="educationStartTime" style="width:240px;">
                    <el-date-picker type="date" placeholder="选择日期" v-model="educationalForm.educationStartTime" style="width: 100%;"></el-date-picker>
                  </el-form-item>
                </el-col>
              </el-form-item><br>

              <el-form-item label="学校名称：" class="el-form-item" >
                <el-col :span="11">
                  <el-form-item prop="educationStudentname" style="width:240px;">
                    <el-input   v-model="educationalForm.educationStudentname" style="width: 100%;"></el-input>
                  </el-form-item>
                </el-col>
              </el-form-item><br>


            </div>

            <div style="display: inline-block;position: absolute;top:20px;right:150px">
              <br>
              <el-form-item label="结束时间:" required >
                <el-col :span="11">
                  <el-form-item prop="educationEndTime" style="width:240px;">
                    <el-date-picker type="date" placeholder="选择日期" v-model="educationalForm.educationEndTime" style="width: 100%;"></el-date-picker>
                  </el-form-item>
                </el-col>
              </el-form-item><br>


              <div style="float: right">
                <el-form-item label="所属专业：" >
                  <el-col :span="11">
                    <el-form-item prop="educationMajor" style="width:240px;">
                      <el-input type="textarea" v-model="educationalForm.educationMajor" style="width: 100%;"></el-input>
                    </el-form-item>
                  </el-col>
                </el-form-item>
              </div>
            </div>


            <div style="width:90%;height:60px;margin: auto;margin-top:30px;padding: 0px 0px 30px 0px">
              <div style="width:20%;height:50px;margin:auto;">
                <el-button @click="educationalCancel()">取消</el-button>
                <el-button type="primary" @click="insertEducation()">保存</el-button>
              </div>
            </div>
          </el-form>
        </div>

      </div>


      <!--      调动记录-->
      <div style="border-left:3px solid blue;margin-top: 20px; ">
        <h3>
          <span style="margin-left:10px;">调动记录</span>
        </h3>
      </div>

      <!--      调动记录信息-->
      <div class="information">
        <h3 style="color: #085fc3;font-size: 14px;margin-left: 10px;display: inline-block;">调动记录信息<i class="iconfont" style="color: #085fc3;margin-right:2px;">&#xe604;</i></h3>
        <div style="width:845px;border-top:1px solid silver;display: inline-block;margin-left: 7px;margin-bottom: 5px;"></div>
      </div>

      <div style="width: 100%;margin-top: 20px;">

        <div style="width:100%;position: relative;margin-top: 20px;" v-show="tranferShow">
          <div v-for="(obj,index) in tranferAll" class="information_text">
            <ul style="list-style-type: none; ">
              <li>
                <label>异动类型</label>
                <p>{{obj.transferType}}</p>
              </li>
              <li>
                <label>原部门名称</label>
                <p>{{obj.deptName}}</p>
              </li>
              <li>
                <label>变动后部门名称</label>
                <p>{{obj.deptName2}}</p>
              </li>
              <li>
                <label>原职位名称</label>
                <p>{{obj.positionName}}</p>
              </li>
              <li>
                <label>变动后部门名称</label>
                <p>{{obj.positionName2}}</p>
              </li>
              <li>
                <label>生效日期</label>
                <p>{{obj.takeEffectDate}}</p>
              </li>

            </ul><br/>
          </div>

        </div>


      </div>


    </div>
  </div>

<!--  //工作经历编辑弹框-->
  <div style="background: #99a9bf">
    <el-dialog
        v-model=" workbecome"
        title="编辑工作经历"
        width="50%"
        :close-on-click-modal="false"
    >
      <el-form style="width: 90%;margin: auto; " :rules="rules" ref="workForm" :model="workForm">
        <br/>
        <div style="display: inline-block;margin:0px 0px 0px -10px">
          <el-form-item label="开始时间:" required >
            <el-col :span="11" >
              <el-form-item prop="start_time" style="width:240px;">
                <el-date-picker type="date" placeholder="选择日期" v-model="workForm.workStareTime" style="width: 100%;"></el-date-picker>
              </el-form-item>
            </el-col>
          </el-form-item><br>

          <el-form-item label="任职公司：" class="el-form-item" >
            <el-col :span="11">
              <el-form-item prop="company" style="width:240px;">
                <el-input   v-model="workForm.companyName" style="width: 100%;"></el-input>
              </el-form-item>
            </el-col>
          </el-form-item><br>
        </div>

        <div style="display: inline-block;position: absolute;top:82px;right:50px">
          <br>
          <el-form-item label="结束时间:" required >
            <el-col :span="11">
              <el-form-item prop="end_time" style="width:240px;">
                <el-date-picker type="date" placeholder="选择日期" v-model="workForm.workEndTime" style="width: 100%;"></el-date-picker>
              </el-form-item>
            </el-col>
          </el-form-item><br>


          <div style="float: right">
            <el-form-item label="职位：" >

              <el-form-item prop="position" style="width:240px;">
                <el-input  v-model="workForm.positionName" style="width: 100%;"></el-input>
              </el-form-item>

            </el-form-item>
          </div>
        </div>

        <div style="width:90%;height:60px;margin: auto;margin-top:30px;padding: 0px 0px 30px 0px">
          <div style="width:30%;height:50px;margin:auto;">
            <el-button @click="workbecome=false;selectWorkId()">取消</el-button>
            <el-button type="primary" @click="updateWork()">保存</el-button>
          </div>
        </div>
      </el-form>

    </el-dialog>


  </div>



<!--  //荣誉与奖励编辑弹框-->
  <div style="background: #99a9bf">
    <el-dialog
        v-model="honorsbecome"
        title="编辑荣誉与奖励"
        width="50%"
        :close-on-click-modal="false"
    >
      <el-form style="width: 90%;margin: auto; " :rules="rules" ref="honorsForm" :model="honorsForm">
        <br/>

        <div style="display: inline-block;margin:0px 0px 0px -10px">
          <el-form-item label="荣誉/奖项名称:" prop="gloryName" >
            <el-input v-model="honorsForm.gloryName" style="width:240px;"></el-input>
          </el-form-item><br>

          <el-form-item label="颁发单位名称：" class="el-form-item" >
            <el-col :span="11">
              <el-form-item prop="gloryUnitname" style="width:240px;">
                <el-input   v-model="honorsForm.gloryUnitname" style="width: 100%;"></el-input>
              </el-form-item>
            </el-col>
          </el-form-item><br>

        </div>

        <div style="display: inline-block;position: absolute;top:82px;right:50px">
          <br>
          <el-form-item label="奖励日期:" >
            <el-col :span="11">
              <el-form-item prop="createdTime" style="width:240px;">
                <el-date-picker type="date" placeholder="选择日期" v-model="honorsForm.createdTime" style="width: 100%;"></el-date-picker>
              </el-form-item>
            </el-col>
          </el-form-item><br>


          <div style="float: right">
            <el-form-item label="备注：" >

              <el-form-item prop="gloryRemark" style="width:240px;">
                <el-input type="textarea"  v-model="honorsForm.gloryRemark" style="width: 100%;"></el-input>
              </el-form-item>

            </el-form-item>
          </div>
        </div>


        <div style="width:90%;height:60px;margin: auto;margin-top:30px;padding: 0px 0px 30px 0px">
          <div style="width:30%;height:50px;margin:auto;">
            <el-button @click="honorsbecome=false,selectGlory()">取消</el-button>
            <el-button type="primary" @click="updateGlory()">保存</el-button>
          </div>
        </div>
      </el-form>

    </el-dialog>


  </div>




  <!--  //惩罚编辑弹框-->
  <div style="background: #99a9bf">
    <el-dialog
        v-model="punishmentbecome"
        title="编辑惩罚"
        width="50%"
        :close-on-click-modal="false"
    >
      <el-form style="width: 90%;margin: auto; " :rules="rules" ref="punishmentForm" :model="punishmentForm">
        <br/>

        <div style="display: inline-block;margin:0px 0px 0px -10px">
          <el-form-item label="惩罚类型:" prop="punishType">
            <el-select v-model="punishmentForm.punishType" placeholder="请选择惩罚类型" style="width: 240px;">
              <el-option label="警告" value="警告"></el-option>
              <el-option label="记过" value="记过"></el-option>
              <el-option label="记大过" value="记大过"></el-option>
              <el-option label="降职" value="降职"></el-option>
              <el-option label="辞退" value="辞退"></el-option>
            </el-select>
          </el-form-item><br>

          <el-form-item label="惩罚单位：" class="el-form-item" >
            <el-col :span="11">
              <el-form-item prop="punishUnit" style="width:240px;">
                <el-input   v-model="punishmentForm.punishUnit" style="width: 100%;"></el-input>
              </el-form-item>
            </el-col>
          </el-form-item><br>

          <el-form-item label="惩罚备注：" >
            <el-form-item prop="punishRemark" style="width:240px;">
              <el-input type="textarea"  v-model="punishmentForm.punishRemark" style="width: 100%;"></el-input>
            </el-form-item>

          </el-form-item>

        </div>

        <div style="display: inline-block;position: absolute;top:82px;right:50px">
          <br>
          <div style="float: right">

            <el-form-item label="惩罚原因：" >
              <el-form-item prop="punishCause" style="width:240px;">
                <el-input   v-model="punishmentForm.punishCause" style="width: 100%;"></el-input>
              </el-form-item>

            </el-form-item><br/>

            <el-form-item label="是否撤销：" >
              <el-radio-group v-model="punishmentForm.isRevocation" style="position: absolute;left:2px;">
                <el-radio :label="0">否</el-radio>
                <el-radio :label="1">是</el-radio>
              </el-radio-group>
            </el-form-item>
          </div>
        </div>


        <div style="width:90%;height:60px;margin: auto;margin-top:30px;padding: 0px 0px 30px 0px">
          <div style="width:30%;height:50px;margin:auto;">
            <el-button @click="punishmentbecome=false;selectPunish()">取消</el-button>
            <el-button type="primary" @click="updatePunish()">保存</el-button>
          </div>
        </div>
      </el-form>

    </el-dialog>


  </div>


  <!--  //教育经历编辑弹框-->
  <div style="background: #99a9bf">
    <el-dialog
        v-model="educationalbecome"
        title="编辑教育经历"
        width="50%"
        :close-on-click-modal="false"
    >
      <el-form style="width: 90%;margin: auto; " :rules="rules" ref="educationalForm" :model="educationalForm">
        <br/>
        <div style="display: inline-block;margin:0px 0px 0px -10px">
          <el-form-item label="开始时间:" required >
            <el-col :span="11" >
              <el-form-item prop="educationStartTime" style="width:240px;">
                <el-date-picker type="date" placeholder="选择日期" v-model="educationalForm.educationStartTime" style="width: 100%;"></el-date-picker>
              </el-form-item>
            </el-col>
          </el-form-item><br>

          <el-form-item label="学校名称：" class="el-form-item" >
            <el-col :span="11">
              <el-form-item prop="educationStudentname" style="width:240px;">
                <el-input   v-model="educationalForm.educationStudentname" style="width: 100%;"></el-input>
              </el-form-item>
            </el-col>
          </el-form-item><br>


        </div>

        <div style="display: inline-block;position: absolute;top:82px;right:50px">
          <br>
          <el-form-item label="结束时间:" required >
            <el-col :span="11">
              <el-form-item prop="educationEndTime" style="width:240px;">
                <el-date-picker type="date" placeholder="选择日期" v-model="educationalForm.educationEndTime" style="width: 100%;"></el-date-picker>
              </el-form-item>
            </el-col>
          </el-form-item><br>


          <div style="float: right">
            <el-form-item label="所属专业：" >
              <el-col :span="11">
                <el-form-item prop="educationMajor" style="width:240px;">
                  <el-input  v-model="educationalForm.educationMajor" style="width: 100%;"></el-input>
                </el-form-item>
              </el-col>
            </el-form-item>
          </div>
        </div>


        <div style="width:90%;height:60px;margin: auto;margin-top:30px;padding: 0px 0px 30px 0px">
          <div style="width:30%;height:50px;margin:auto;">
            <el-button @click="educationalbecome=false">取消</el-button>
            <el-button type="primary" @click="updateEducation()">保存</el-button>
          </div>
        </div>
      </el-form>

    </el-dialog>


  </div>


</template>

<script>
import { defineComponent, ref } from 'vue'
import {ElMessage} from "element-plus";

export default {
  data() {
    return {
      //显示调动记录信息
      tranferShow:true,
      //工作经历弹框
      workbecome:false,
      honorsbecome:false,
      punishmentbecome:false,
      educationalbecome:false,

      // 工作经历 all
      workAll:[],
      // 荣誉与奖励 all
      honorsAll:[],
      //惩罚
      punishmentAll:[],
      //教育经历
      educationalAll:[],

     //工作经历
      workForm: {
        //开始时间
        workStareTime:'',
        //任职公司
        companyName:'',
        //职位
        positionName:'',
        //结束时间
        workEndTime:'',

      },
      rules: {
        workStareTime: [
          { type: 'date', required: true, message: '请选择日期', trigger: 'blur' }
        ],
        workEndTime: [
          { type: 'date', required: true, message: '请选择时间', trigger: 'blur' }
        ],
      },

      //工作经历右上角的添加按钮
      workerButton:false,
      //添加 工作经历
      addWork:true,
      //显示工作经历信息
      workShow:false,
      //显示编辑的input表单
      workInput:false,

      //荣誉与奖励
      honorsForm:{
        //荣誉与奖励名称
        gloryName:'',
        //颁发单位名称
        gloryUnitname:'',
        //奖励日期
        createdTime:'',
        //备注
        gloryRemark:'',
      },

      //荣誉与奖励右上角的添加按钮
      honorsButton:false,
      //添加 荣誉与奖励
      addHonors:true,
      //显示荣誉与奖励信息
      honorsShow:false,
      //显示编辑的input表单
      honorsInput:false,

      //惩罚
      punishmentForm:{
        //惩罚类型
        punishType:'',
        //惩罚单位
        punishUnit:'',
        //惩罚备注
        punishRemark:'',
        //惩罚原因
        punishCause:'',
        //是否撤销
        isRevocation:'',
      },

      //惩罚右上角的添加按钮
      punishmentButton:false,
      //添加 惩罚
      addPunishment:true,
      //显示惩罚信息
      punishmentShow:false,
      //显示编辑的input表单
      punishmentInput:false,

      //教育经历
      educationalForm:{
        //开始时间
        educationStartTime:'',
        //学校名称
        educationStudentname:'',
        //结束时间
        educationEndTime:'',
        //所属专业
        educationMajor:'',
      },

      //教育经历右上角的添加按钮
      educationalButton:false,
      //添加 教育经历
      addEducational:true,
      //显示教育经历信息
      educationalShow:false,
      //显示编辑的input表单
      educationalInput:false,

      //调动记录表
      tranferAll:{
        //异动类型
        transferType:'' ,
        //原部门名称
        deptName:'' ,
        //变动后部门名称
        deptName2:'',
        positionName:'',//原部门职位名称
        positionName2:'',// 变动后部门职位名称
        takeEffectDate:'',//生效日期
      },



    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!');
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },


    //点击个人信息 显示
    childrenWork(){
      //当workForm中有值时，显示出来
      if(this.workAll.length>0){
        //显示工作经历信息
        this.workShow=true
        //隐藏 添加工作经历框
        this.addWork=false
        //显示右上角添加按钮
        this.workerButton=true

      }else {
        //显示工作经历信息
        this.workShow = false
        //隐藏 添加工作经历框
        this.addWork = true
        //显示右上角添加按钮
        this.workerButton = false
      }

        if(this.honorsAll.length>0){
          //显示荣誉/奖励信息
          this.honorsShow=true
          //隐藏 添加荣誉/奖励框
          this.addHonors=false
          //显示右上角添加按钮
          this.honorsButton=true
        }else{
          //显示荣誉/奖励信息
          this.honorsShow=false
          //隐藏 添加荣誉/奖励框
          this.addHonors=true
          //显示右上角添加按钮
          this.honorsButton=false
        }

        if(this.punishmentAll.length>0){
          //显示惩罚信息
          this.punishmentShow=true
          //隐藏 添加惩罚框
          this.addPunishment=false
          //显示右上角添加按钮
          this.punishmentButton=true
        }else{
          //显示惩罚信息
          this.punishmentShow=false
          //隐藏 添加惩罚框
          this.addPunishment=true
          //显示右上角添加按钮
          this.punishmentButton=false

        }

        if(this.educationalAll.length>0){
          //显示教育经历信息
          this.educationalShow=true
          //隐藏 添加教育经历框
          this.addEducational=false
          //显示右上角添加按钮
          this.educationalButton=true
        }else{
          //显示教育经历信息
          this.educationalShow=false
          //隐藏 添加教育经历框
          this.addEducational=true
          //显示右上角添加按钮
          this.educationalButton=false
        }

      this.tranferShow=true;


    },
    // 添加 工作经历
    addWorks(){

      this.workInput=true // 显示灰色 表单
      this.addWork=false // 隐藏 添加工作经历
    },

    // 工作经历表单取消按钮
    workCancel(){

      if(this.workAll.length==0){ // 没有工作经历信息时
        this.addWork=true // 显示 添加工作经历
      }

      this.workInput=false // 隐藏灰色 表单

      //清空开始时间
      this.workForm.workStareTime="";
      //清空任职公司
      this.workForm.companyName="";
      //清空职位
      this.workForm.positionName="";
      //清空结束时间
      this.workForm.workEndTime="";
    },

    // 工作经历表单保存按钮
    workSave(){

      this.workInput=false // 隐藏灰色 表单
      this.workShow=true // 显示所有工作经历信息
      this.workerButton=true // 显示右上角添加按钮

      // 清空表单数据  this.xxx=null or =""
      //清空开始时间
      this.workForm.workStareTime="";
      //清空任职公司
      this.workForm.companyName="";
      //清空职位
      this.workForm.positionName="";
      //清空结束时间
      this.workForm.workEndTime="";

    },

    // 右上角的添加按钮
    addButton(){
      this.workInput=true // 显示灰色 表单

      //清空开始时间
      this.workForm.workStareTime="";
      //清空任职公司
      this.workForm.companyName="";
      //清空职位
      this.workForm.positionName="";
      //清空结束时间
      this.workForm.workEndTime="";
    },


    // 工作经历编辑按钮
    workEditor(i){
      // 根据id取值 赋值到弹出框
      this.workForm=this.workAll[i]
      // 打开弹出框
      this.workbecome=true

    },

    // 工作经历删除按钮
     workDelete(){
      /*
       splice（i,n） : 按照下标删除数组里的元素 从i开始删除n个元素
       i: 下标
       n: 删除个数
       */

       // 当工作经历信息删除为空时(数组长度为0时)
       if(this.workAll.length==0){
         this.addWork=true // 添加工作经历按钮显示
         this.workerButton=false // 右上角添加按钮隐藏
       }
     },


    // 添加 荣誉与奖励
    addHonorss(){
      this.honorsInput=true // 显示灰色 表单
      this.addHonors=false // 隐藏 添加工作经历
    },

    // 荣誉与奖励表单取消按钮
    honorsCancel(){

      if(this.honorsAll.length==0){ // 没有工作经历信息时
        this.addHonors=true // 显示 添加工作经历
      }

      this.honorsInput=false // 隐藏灰色 表单

      // 清空表单数据  this.xxx=null or =""
      //清空荣誉与奖励名称
      this.honorsForm.gloryName="";
      //清空颁发单位名称
      this.honorsForm.gloryUnitname="";
      //清空奖励日期
      this.honorsForm.createdTime="";
      //清空备注
      this.honorsForm.gloryRemark="";

    },

    // 荣誉与奖励表单保存按钮
    honorsSave(){
      this.honorsInput=false // 隐藏灰色 表单
      this.honorsShow=true // 显示所有工作经历信息
      this.honorsButton=true // 显示右上角添加按钮


    },

    //右上角的添加按钮
    addhonorsButton(){
      //清空荣誉与奖励名称
      this.honorsForm.gloryName="";
      //清空颁发单位名称
      this.honorsForm.gloryUnitname="";
      //清空奖励日期
      this.honorsForm.createdTime="";
      //清空备注
      this.honorsForm.gloryRemark="";
      this.honorsInput=true // 显示灰色 表单



    },

    // 荣誉与奖励编辑按钮
    honorsEditor(i){


      //编辑荣誉与奖励名称
      this.honorsForm=this.honorsAll[i]
      this.honorsbecome=true


    },

    // 工作经历删除按钮
    honorsDelete(i){
      /*
       splice（i,n） : 按照下标删除数组里的元素 从i开始删除n个元素
       i: 下标
       n: 删除个数
       */

      this.honorsAll.splice(i,1)
      // 当工作经历信息删除为空时(数组长度为0时)
      if(this.honorsAll.length==0){
        this.addHonors=true // 添加工作经历按钮显示
        this.honorsButton=false // 右上角添加按钮隐藏
      }
    },





    // 惩罚表单保存按钮
    punishSave(){
      this.punishmentInput=false // 隐藏灰色 表单
      this.punishmentShow=true // 显示所有工作经历信息
      this.punishmentButton=true // 显示右上角添加按钮


    },

    // 添加 惩罚
    addPunishments(){
      this.punishmentInput=true // 显示灰色 表单
      this.addPunishment=false // 隐藏 添加惩罚
    },

    // 惩罚表单取消按钮
    punishmentCancel(){

      if(this.punishmentAll.length==0){ // 没有惩罚信息时
        this.addPunishment=true // 显示 添加惩罚
      }

      this.punishmentInput=false // 隐藏灰色 表单

      // 清空表单数据  this.xxx=null or =""

      //清空惩罚类型
      this.punishmentForm.punishType="";
      //清空惩罚单位
      this.punishmentForm.punishUnit="";
      //清空惩罚备注
      this.punishmentForm.punishRemark="";
      //清空惩罚原因
      this.punishmentForm.punishCause="";
      //清空是否撤销
      this.punishmentForm.isRevocation="";


    },


    // 右上角的添加按钮
    addpunishmentButton(){
      this.punishmentInput=true // 显示灰色 表单

      //清空惩罚类型
      this.punishmentForm.punishType="";
      //清空惩罚单位
      this.punishmentForm.punishUnit="";
      //清空惩罚备注
      this.punishmentForm.punishRemark="";
      //清空惩罚原因
      this.punishmentForm.punishCause="";
      //清空是否撤销
      this.punishmentForm.isRevocation="";
    },

    // 荣誉与奖励编辑按钮
    punishmentEditor(i){

      this.punishmentForm=this.punishmentAll[i]
      this.punishmentbecome=true;

    },

    // 工作经历删除按钮
    punishmentDelete(i){
      /*
       splice（i,n） : 按照下标删除数组里的元素 从i开始删除n个元素
       i: 下标
       n: 删除个数
       */

      this.punishmentAll.splice(i,1)
      // 当工作经历信息删除为空时(数组长度为0时)
      if(this.punishmentAll.length==0){
        this.addPunishment=true // 添加工作经历按钮显示
        this.punishmentButton=false // 右上角添加按钮隐藏
      }
    },





    // 添加 教育经历
    addeducationals(){
      this.educationalInput=true // 显示灰色 表单
      this.addEducational=false // 隐藏 添加工作经历
    },

    // 教育经历表单取消按钮
    educationalCancel(){

      if(this.educationalAll.length==0){ // 没有教育经历信息时
        this.addEducational=true // 显示 添加教育经历
      }

      this.educationalInput=false // 隐藏灰色 表单

      // 清空表单数据  this.xxx=null or =""

      //清空开始时间
      this.educationalForm.educationStartTime="";
      //清空学校名称
      this.educationalForm.educationStudentname="";
      //清空结束时间
      this.educationalForm.educationEndTime="";
      //清空所属专业
      this.educationalForm.educationMajor="";


    },

    // 教育经历表单保存按钮
    educationalSave(){

      // 向数组里面添加 表单数据
      this.educationalAll.push(formeducat)

      this.educationalInput=false // 隐藏灰色 表单
      this.educationalShow=true // 显示所有教育经历信息
      this.educationalButton=true // 显示右上角添加按钮

      // 清空表单数据  this.xxx=null or =""
      //清空开始时间
      this.educationalForm.educationStartTime="";
      //清空学校名称
      this.educationalForm.educationStudentname="";
      //清空结束时间
      this.educationalForm.educationEndTime="";
      //清空所属专业
      this.educationalForm.educationMajor="";


    },

    // 右上角的添加按钮
    addeducationalButton(){
      this.educationalInput=true // 显示灰色 表单
    },

    // 教育经历编辑按钮
    educationalEditor(i){

      this.educationalForm=this.educationalAll[i],
      this.educationalbecome=true


    },

    // 教育经历删除按钮
    educationalDelete(i){
      /*
       splice（i,n） : 按照下标删除数组里的元素 从i开始删除n个元素
       i: 下标
       n: 删除个数
       */

      this.educationalAll.splice(i,1)
      // 当教育经历信息删除为空时(数组长度为0时)
      if(this.educationalAll.length==0){
        this.addEducational=true // 添加工作经历按钮显示
        this.educationalButton=false // 右上角添加按钮隐藏
      }
    },
    //根据id查询员工信息
    selectWorkId(){

        this.axios
            .get("http://localhost:8010/provider/workExperience/selectWorkExperienceId/" + this.$store.state.staffId_Msg)
            .then((response) => {
              console.log(response);
              this.workAll = response.data.data;
              this.childrenWork();

            })
            .catch(function (error) {
              console.log(error);
            });

    },
    // 修改工作经历
    updateWork(){
      this.axios({
        url: 'http://localhost:8010/provider/workExperience/updateWorkExperienceId',
        method: 'put',
        data: this.workForm
      }).then(response => {
        console.log(response)
        if (response.data.data >0) {
          ElMessage({
            message: '修改成功',
            type: 'success',
          })
          //调用查询工作经历
          this.selectWorkId()
          //关闭弹出框
          this.workbecome=false
        } else {
          ElMessage.error('修改失败')
        }
      }).catch(function (error) {
        console.log(error);
      });
    },
    //根据id删除工作经历
    deleteWorkId(index){

      this.axios
          .delete("http://localhost:8010/provider/workExperience/deleteWorkExperienceId/"+index.workExperienceId)
          .then((response) => {
            console.log(response);

            if (response.data.data >0) {
              ElMessage({
                message: '删除成功',
                type: 'success',
              })
              //调用查询工作经历
              this.selectWorkId()


            } else {
              ElMessage.error('删除失败')
            }
          })
          .catch(function (error) {
            console.log(error);
          });

    },
    //添加工作经历
    insertWork(){
      this.axios({
        url: 'http://localhost:8010/provider/workExperience/insertWorkExperience',
        method: 'post',
        data:{
          staffId:this.$store.state.staffId_Msg,
          workStareTime:this.workForm.workStareTime,
          workEndTime:this.workForm.workEndTime,
          companyName:this.workForm.companyName,
          positionName:this.workForm.positionName
        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.selectWorkId() // 修改完成后调用查询方法
          this.workSave()
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
        console.log(error);
      });

    },

    //查询荣誉/奖励信息
    selectGlory(){

      this.axios
          .get("http://localhost:8010/provider/Glory/selectGloryId/" + this.$store.state.staffId_Msg)
          .then((response) => {
            console.log(response);
            this.honorsAll = response.data.data;
            this.childrenWork();

          })
          .catch(function (error) {
            console.log(error);
          });


    },
    //修改荣誉/奖励信息
    updateGlory(){
      this.axios({
        url: 'http://localhost:8010/provider/Glory/updateGlory',
        method: 'put',
        data: this.honorsForm
      }).then(response => {
        console.log(response)
        if (response.data.data >0) {
          ElMessage({
            message: '修改成功',
            type: 'success',
          })
          //调用查询工作经历
          this.selectGlory()
          //关闭弹出框
          this.honorsbecome=false
        } else {
          ElMessage.error('修改失败')
        }
      }).catch(function (error) {
        console.log(error);
      });
    },
    //添加荣誉/奖励信息
    insertGlory(){

      this.axios({
        url: 'http://localhost:8010/provider/Glory/insertGlory',
        method: 'post',
        data:{
          //员工编号
          staffId:this.$store.state.staffId_Msg,
          //荣誉与奖励名称
          gloryName:this.honorsForm.gloryName,
          //颁发单位名称
          gloryUnitname:this.honorsForm.gloryUnitname,
          //奖励日期
          createdTime:this.honorsForm.createdTime,
          //备注
          gloryRemark:this.honorsForm.gloryRemark,

        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.selectGlory() // 修改完成后调用查询方法
          this.honorsSave()
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
        console.log(error);
      });

    },
    //根据id删除荣誉/奖励信息
    deleteGloryId(index){
      this.axios
          .delete("http://localhost:8010/provider/Glory/deleteGloryId/"+index.gloryId)
          .then((response) => {
            console.log(response);

            if (response.data.data >0) {
              ElMessage({
                message: '删除成功',
                type: 'success',
              })
              //调用查询工作经历
              this.selectGlory()


            } else {
              ElMessage.error('删除失败')
            }
          })
          .catch(function (error) {
            console.log(error);
          });

    },
    //查询惩罚表信息
    selectPunish(){

      this.axios
          .get("http://localhost:8010/provider/Punish/selectPunishId/" + this.$store.state.staffId_Msg)
          .then((response) => {
            console.log(response);
            this.punishmentAll = response.data.data;
            this.childrenWork();

          })
          .catch(function (error) {
            console.log(error);
          });


    },
    //修改惩罚信息
    updatePunish(){
      this.axios({
        url: 'http://localhost:8010/provider/Punish/updatePunish',
        method: 'put',
        data: this.punishmentForm
      }).then(response => {
        console.log(response)
        if (response.data.data >0) {
          ElMessage({
            message: '修改成功',
            type: 'success',
          })
          //调用查询工作经历
          this.selectPunish()
          //关闭弹出框
          this.punishmentbecome=false
        } else {
          ElMessage.error('修改失败')
        }
      }).catch(function (error) {
        console.log(error);
      });
    },
    //添加荣誉/奖励信息
    insertPunish(){

      this.axios({
        url: 'http://localhost:8010/provider/Punish/insertPunish',
        method: 'post',
        data:{
          //员工编号
          staffId:this.$store.state.staffId_Msg,
          //惩罚类型
          punishType:this.punishmentForm.punishType,
          //惩罚单位
          punishUnit:this.punishmentForm.punishUnit,
          //惩罚备注
          punishRemark:this.punishmentForm.punishRemark,
          //惩罚原因
          punishCause:this.punishmentForm.punishCause,
          //是否撤销
          isRevocation:this.punishmentForm.isRevocation,


        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.selectPunish() // 修改完成后调用查询方法
          this.punishSave()
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
        console.log(error);
      });

    },
    //根据id删除荣誉/奖励信息
    deletePunishId(index){
      this.axios
          .delete("http://localhost:8010/provider/Punish/deletePunishId/"+index.punishId)
          .then((response) => {
            console.log(response);

            if (response.data.data >0) {
              ElMessage({
                message: '删除成功',
                type: 'success',
              })
              //调用查询工作经历
              this.selectPunish()


            } else {
              ElMessage.error('删除失败')
            }
          })
          .catch(function (error) {
            console.log(error);
          });

    },
    //查询教育经历表信息
    selectEducation(){

      this.axios
          .get(" http://localhost:8010/provider/education/selectEducationId/" + this.$store.state.staffId_Msg)
          .then((response) => {
            console.log(response);
            this.educationalAll = response.data.data;
            this.childrenWork();

          })
          .catch(function (error) {
            console.log(error);
          });


    },
    //修改教育经历信息
    updateEducation(){
      this.axios({
        url: 'http://localhost:8010/provider/education/updateEducation',
        method: 'put',
        data: this.educationalForm
      }).then(response => {
        console.log(response)
        if (response.data.data >0) {
          ElMessage({
            message: '修改成功',
            type: 'success',
          })
          //调用查询工作经历
          this.selectEducation()
          //关闭弹出框
          this.educationalbecome=false
        } else {
          ElMessage.error('修改失败')
        }
      }).catch(function (error) {
        console.log(error);
      });
    },
    //添加教育经历信息
    insertEducation(){

      this.axios({
        url: 'http://localhost:8010/provider/education/insertEducation',
        method: 'post',
        data:{
          //员工编号
          staffId:this.$store.state.staffId_Msg,

          //开始时间
          educationStartTime:this.educationalForm.educationStartTime,
          //学校名称
          educationStudentname:this.educationalForm.educationStudentname,
          //结束时间
          educationEndTime:this.educationalForm.educationEndTime,
          //所属专业
          educationMajor:this.educationalForm.educationMajor,


        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.selectEducation() // 修改完成后调用查询方法
          this.educationalSave()
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
        console.log(error);
      });

    },
    //根据id删除教育经历信息
    deleteEducationId(index){
      this.axios
          .delete("http://localhost:8010/provider/education/deleteEducationId/"+index.educationId)
          .then((response) => {
            console.log(response);

            if (response.data.data >0) {
              ElMessage({
                message: '删除成功',
                type: 'success',
              })
              //调用查询工作经历
              this.selectEducation()


            } else {
              ElMessage.error('删除失败')
            }
          })
          .catch(function (error) {
            console.log(error);
          });

    },
    //查询调动记录
    selectTranferId(){

      this.axios
          .get("http://localhost:8010/provider/transfer/selectTransferId/" + this.$store.state.staffId_Msg)
          .then((response) => {
            console.log(response);
            this.tranferAll = response.data.data;


          })
          .catch(function (error) {
            console.log(error);
          });


    },
  },created() {
    this.selectWorkId()
    this.selectGlory()
    this.selectPunish()
    this.selectEducation()
    this.selectTranferId()
  }


}
</script>

<style scoped>
/*@import url("../../css/EditingStaff.css");*/
  .basicfile_big{
    width:100%;
    /*border:1px solid red;*/
    margin-top:20px;
    margin: auto;
  }
  .basicfile_big_auto{
    width:80%;
    /*border:1px solid yellow;*/
    margin: auto;
  }
  .information{
    position: relative;
    margin-top: 10px;
    width:100%;
    margin: auto;
  }
  .information_text{
    width:850px;
    margin: 0px auto;
    /*background: darkorange;*/
  }
  .information_text li{
    /*float: left;*/
    display: inline-block;
    width:50%;
    padding: 4px 0px;

  }
  .information_text li label{
    width: 200px;
    display: inline-block;
    text-align: right;
    vertical-align: middle;
    position: relative;
    color: #969799;
    font-size: 14px;
    padding: 0 24px 0 0;
    box-sizing: border-box;
  }
  .information_text li p{
    display: inline-block;
  }

  .information_from{
    width:100%;
    background: #e8edf2;
    position: relative;

  }

  .information_ul li{
    display: inline-block;
    width:50%;
    padding: 20px 65px 0px;
    text-align: right;

  }

  .information_ul li label{
    /*width:100px;*/
    text-align: right;
    vertical-align: middle;
    position: relative;
    color: rgba(0, 0, 0, 0.85);
    font-size: 14px;
    /*padding: 0 24px 0 0;*/
    box-sizing: border-box;
  }

  .information_ul{
    list-style-type: none;
  }
  .information_input{
    width:240px;
    display: inline-block;
  }
  .el-form-item{
    text-align: right;
  }

</style>