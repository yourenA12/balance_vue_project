<!--招聘过程 人才库-->
<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body ">
        <div class="j-tabs">



            <el-tabs type="border-card">
              <el-tab-pane>
                <template #label>
                  <router-link :to="{path:this.one,query:{path:this.$route.query.path}}">
                    <span>全部简历&nbsp;&nbsp;&nbsp;&nbsp;</span>
                  </router-link>
                </template>
                <router-view/>
              </el-tab-pane>

              <el-tab-pane>
                <template #label>
                  <router-link :to="{path:this.two,query:{path:this.$route.query.path}}">
                    <span v-bind:class="{sactive:isActive2}">新简历&nbsp;&nbsp;</span>
                  </router-link>
                </template>
                <router-view/>
              </el-tab-pane>

              <el-tab-pane>
                <template #label>
                  <router-link :to="{path:this.three,query:{path:this.$route.query.path}}">
                    <span v-bind:class="{sactive:isActive3}">候选人&nbsp;&nbsp;&nbsp;</span>
                  </router-link>
                </template>
                <router-view/>
              </el-tab-pane>

              <el-tab-pane>
                <template #label>
                  <router-link :to="{path:this.foue,query:{path:this.$route.query.path}}">
                    <span v-bind:class="{sactive:isActive4}">淘汰库&nbsp;&nbsp;&nbsp;</span>
                  </router-link>
                </template>
                <router-view/>
              </el-tab-pane>
            </el-tabs>

        </div>
        <div>
        </div>




      </div>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      //路由地址
      one:'/recruitment/recruit/fullresume',
      two:'/recruitment/recruit/newresume',
      three:'/recruitment/recruit/candidate',
      foue:'/recruitment/recruit/eliminate'

    }
  },
  methods:{

  }
}

</script>

<style  type="text/css" scoped>
@import url("../../css/navigation.css");
@import url("../../css/zpdaohang.css");

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




</style>
