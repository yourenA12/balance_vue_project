<!-- 简历赛选 -->
<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body ">
        <div class="j-tabs">

          <el-tabs type="border-card">
            <el-tab-pane>
              <template #label>
                <router-link :to="{path:this.one,query:{path:this.$route.query.path}}">
                  <span v-bind:class="{sactive:isActive1}">候选人</span>
                </router-link>
              </template>
              <router-view/>
            </el-tab-pane>




            <el-tab-pane>
              <template #label>
                <router-link :to="{path:this.two,query:{path:this.$route.query.path}}">
                  <span v-bind:class="{sactive:isActive2}">已邀约</span>
                </router-link>
              </template>
              <router-view/>
            </el-tab-pane>
          </el-tabs>
        </div>
<!--        <div>-->
<!--          <router-view/>-->
<!--        </div>-->




      </div>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      isActive1:true,
      isActive2:false,
      //路由地址
      one:'/recruitment/candidate',
      two:'/recruitment/Invited',

    }
  },
  methods:{
    liback(n){
      if(n===1){
        this.isActive1=true;
        this.isActive2=false;
        this.isActive3=false;
        this.isActive4=false;
      }
      if(n===2){
        this.isActive1=false;
        this.isActive2=true;
        this.isActive3=false;
        this.isActive4=false;
      }

    }
  }
}

</script>

<style  type="text/css" scoped>
@import url("../../css/navigation.css");
@import url("../../css/zpdaohang.css");
.j-tabs {
  border-bottom: 1px solid #d9d9d9;
}
.j-tabs ul {
  margin-bottom: -2px;
  padding-left: 0px;
}
.j-tabs li {
  margin: 0;
  display: inline-block;
  -webkit-transition: all 0.3s ease;
  transition: all 0.3s ease;
  cursor: pointer;
  border: 0;
}
.j-tabs li a {
  display: inline-block;
  height: 54px;
  line-height: 54px;
  color: #000;
  padding: 0 21px;
  font-size: 14px;
}
.active{
  border-bottom: 1px solid #085fc3;
}
.sactive{
  color: #085fc3;
}


</style>
