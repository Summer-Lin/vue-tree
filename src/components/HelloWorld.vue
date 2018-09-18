<template>
  <div class="hello">
    <tree
      :treeData="treeData"
      v-model="ids"
      :withoutParents="withoutParents"
      :options="treeOptions"
    ></tree>


    <div style="margin-top: 100px;">
      包含父节点的数组用这个ids[必填项]:{{ids}}
    </div>
    <div>
      不包含父节点的数组用这个withoutParents[非必填项]:{{withoutParents}}
    </div>
  </div>
</template>

<script>
import tree from '@/components/tree'
export default {
  name: 'HelloWorld',
  data () {
    return{
      ids:[400], //包含父节点
      withoutParents:[400],  //不包含父节点
      treeData:[
        {
          "id": 1,
          "parentId": 0,
          "name": "优订购代发",
          "child": [
            {
              "id": 34,
              "parentId": 1,
              "name": "sea仓库商品管理",
              "child": []
            },
            {
              "id": 343,
              "parentId": 1,
              "name": "sea仓库商品管理",
              "child": []
            },
            {
              "id": 31,
              "parentId": 1,
              "name": "sea仓库门店管理",
              "child": [
                {
                  "id": 33,
                  "parentId": 31,
                  "name": "sea仓库财务管理",
                  "child": []
                },
                {
                  "id": 32,
                  "parentId": 31,
                  "name": "sea仓库门店管理类表",
                  "child": [
                    {
                      "id": 400,
                      "parentId": 32,
                      "name": "sea11111111",
                      "child": []
                    },
                  ]
                }

              ]
            },

            {
              "id": 35,
              "parentId": 1,
              "name": "sea仓库订单管理",
              "child": [
                {
                  "id": 38,
                  "parentId": 35,
                  "name": "sea仓库代发管理",
                  "child": [
                    {
                      "id": 60,
                      "parentId": 35,
                      "name": "sea仓库代发管理",
                      "child": []
                    },
                    {
                      "id": 61,
                      "parentId": 35,
                      "name": "sea仓库代发管理",
                      "child": []
                    }
                  ]
                }
              ]
            },
            {
              "id": 39,
              "parentId": 1,
              "name": "sea仓库个人中心",
              "child": [
                {
                  "id": 40,
                  "parentId": 39,
                  "name": "sea仓库个人信息",
                  "child": []
                },
                {
                  "id": 41,
                  "parentId": 39,
                  "name": "sea仓库账号安全",
                  "child": []
                },
                {
                  "id": 42,
                  "parentId": 39,
                  "name": "sea仓库消息通知",
                  "child": []
                },
                {
                  "id": 43,
                  "parentId": 39,
                  "name": "sea仓库支付管理",
                  "child": []
                }
              ]
            }
          ]
        }
      ],
      treeOptions:{
        withParents:false  //是否包含父节点，如果为false,则要添加withoutParents这个参数，如果为true则不需要添加
      }
    }
  },
  components: {
    'tree':tree
  }
}
</script>

<style>
  li{list-style:none;vertical-align:top;}
</style>
