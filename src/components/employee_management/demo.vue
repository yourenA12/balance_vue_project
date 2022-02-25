<template>

  <!--
    show-checkbox  是否显示多选框
    :default-expand-all=true  是否默认展开
    :check-on-click-node=true 是否在点击节点的时候选中节点
    :check-strictly=true 是否父子联动
   -->

  <!--

  单选：去掉 show-checkbox

  多选：去掉  :check-strictly=true
     // 将差集赋值上选择器
     this.$refs.tree.setCheckedKeys([arr], false)
  -->

  111multiple
  <el-select v-model="depto" ref="vueSelecto" @click="onclickso()">
    <el-option hidden></el-option>
    <el-tree :data="deptlistso"
             show-checkbox
             :default-expand-all=true
             :check-on-click-node=true
             :check-strictly=true
             node-key="deptId"
             :props="defaultProps" ref="treeo" @check-change="handleCheckChangeo()" />
  </el-select>

  {{reso}}

  <br /><br />

  {{res1o}}<br />
  {{res2o}}

</template>

<script>
export default {

  data() {
    // 格式
    const defaultProps = {
      children: 'treeList',
      label: 'deptName',
      value:'deptId'
    }
    return {
      reso:"",
      // 选中值1
      res1o:"",
      // 选中值2
      res2o:"",
      // 部门  文本框的值
      depto:"",
      // 格式
      defaultProps,
      //存放部门信息
      deptlistso: [{
        "deptId": 1,
        "deptPid": 0,
        "deptState": 0,
        "deptName": "华中",
        "treeList": [{
          "deptId": 4,
          "deptPid": 1,
          "deptState": 0,
          "deptName": "华中1",
          "treeList": [{
            "deptId": 10,
            "deptPid": 4,
            "deptState": 0,
            "deptName": "华中1-1",
            "treeList": [{
              "deptId": 12,
              "deptPid": 10,
              "deptState": 0,
              "deptName": "华中1-1-1",
              "treeList": [{
                "deptId": 13,
                "deptPid": 12,
                "deptState": 0,
                "deptName": "华中1-1-1-1",
                "treeList": null
              }]
            }]
          },
            {
              "deptId": 11,
              "deptPid": 4,
              "deptState": 0,
              "deptName": "华中1-2",
              "treeList": null
            }
          ]
        },
          {
            "deptId": 5,
            "deptPid": 1,
            "deptState": 0,
            "deptName": "华中2",
            "treeList": null
          }
        ]
      },
        {
          "deptId": 2,
          "deptPid": 0,
          "deptState": 0,
          "deptName": "华南",
          "treeList": [{
            "deptId": 6,
            "deptPid": 2,
            "deptState": 0,
            "deptName": "华南1",
            "treeList": null
          },
            {
              "deptId": 7,
              "deptPid": 2,
              "deptState": 0,
              "deptName": "华南2",
              "treeList": null
            },
            {
              "deptId": 8,
              "deptPid": 2,
              "deptState": 0,
              "deptName": "华南3",
              "treeList": null
            }
          ]
        },
        {
          "deptId": 3,
          "deptPid": 0,
          "deptState": 0,
          "deptName": "华北",
          "treeList": [{
            "deptId": 9,
            "deptPid": 3,
            "deptState": 0,
            "deptName": "华北1",
            "treeList": null
          }]
        }
      ],

    }
  },
  methods: {

    onclickso() {
      // 点击文本框时调用的方法
      // 取当前选择器中的复选框选项id
      this.res1o = this.$refs.treeo.getCheckedKeys()
    },

    //节点选中状态发生变化时的回调
    handleCheckChangeo(data, checked, indeterminate) {

      // 节点选中状态发生变化
      // 取当前选择器中的复选框选项id
      this.res2o = this.$refs.treeo.getCheckedKeys()

      // 取差集
      let a=new Set(this.res1o);
      let b=new Set(this.res2o);
      let arr = Array.from(new Set([...b].filter(x => !a.has(x))));

      // 将差集赋值上选择器
      this.$refs.treeo.setCheckedKeys([arr], false)

      //获取所有选中的节点 start
      this.reso = this.$refs.treeo.getCheckedNodes()
      this.reso.forEach((item) => {
        // 赋值到文本框
        this.depto = item.deptName
        // 关闭选择器
        this.$refs.vueSelecto.blur();
      })
    },
  }
}
</script>
