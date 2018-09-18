# vue-tree

> A simple tree component for vue.js
>
首先，感谢[JiaxinCui](https://github.com/jiaxincui)，看了他的树形组件源码，按照他的代码和思路，模仿了一个
基于elementUI的横向树形组件用于公司的项目，附上他的项目代码[MIT](https://github.com/jiaxincui/vue-tree/blob/master/LICENSE.md) 

####Demo图片：
![Image text](https://github.com/Summer-Lin/vue-tree/blob/master/src/assets/demo.png)

## 演示

A [Demo](https://jiaxincui.github.io/vue-tree/dist/)

#### 项目跑起来,输入网址http://localhost:8080/#/

```
# install
npm install
npm run dev

```
### treeData示例
./tree.json
```json
 其中，child即使是空数组，也要存在，并且，id不能重复

 treeData:[
        {
          "id": 1,
          "name": "优订购代发",
          "child": [
            {
              "id": 34,
              "name": "sea仓库商品管理",
              "child": []
            }
          ]
        }
 ]
```

## 使用示例
    `App.vue`

      ```vue
      <template>
        <div id="app">
           <tree
                :treeData="treeData"
                v-model="ids"
                :withoutParents="withoutParents"
                :options="treeOptions"
              ></tree>
      </template>

      <script>
      import tree from '@/components/tree'

      export default {
        name: 'app',
        components: {  'tree':tree },
        data () {
          return {
             //包含父节点,必填项
             ids:[400],
              //不包含父节点，非必填项，当treeOptions对象中的withParents为false必须填,
              //而且初始数值跟ids一样
             withoutParents:[400],
              //treeOptions非必填项
             treeOptions:{
              //是否包含父节点，如果为false,则要添加withoutParents这个参数，如果为true则不需要添加
               withParents:false
             },
             数据结构在上面
             treeData:[]
          }
        }
      }
      </script>
      ```

## 设置选项

### treeOptions设置说明

|名称|类型|默认值|说明|
|----|----|----|----|
|`withParents`|`Boolean`|`true`| 当为true时则不需要填写withoutParents参数，当为false时，withoutParents初始值要跟ids一样|

> -- 注意：本组件采用vue+elementUI，使用的是checkbox的样式


## License
[MIT](https://github.com/jiaxincui/vue-tree/blob/master/LICENSE.md) ©
