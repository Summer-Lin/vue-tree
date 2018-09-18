<template>
    <li class="tree" :class="{'sub_tree_all': model.child.length == 0? true : false }">
      <div class="single_btn" v-if="model.child && model.child.length == 0">
        <el-checkbox v-model="model.checked" @change="toggleChange">
          {{model.name}}[{{model.id}}]
        </el-checkbox>
      </div>
      <div v-else>
        <span>{{model.name}}</span>
        <el-checkbox v-model="model.checked" :indeterminate="model.indeterminate" @change="toggleChange">
          全选[{{model.id}}]
        </el-checkbox>
      </div>

      <ul>
        <tree-node
          v-for="(subModel , index) in model.child"
          :model="subModel"
          :ids="ids"
          :key="index"
          :root="root"
          :status="itemStatus"
          :withoutParents="withoutParents"
          :options="options"
          @onChildChange="onChildChange"
        ></tree-node>

      </ul>
    </li>
</template>
<script>
  /**
   *时间：2018/09/15
   *作者:Summer
   *功能:
   *
   * @参数：
   *
   */
  export default {
    name: 'TreeNode',
    props: {
      treeData:{
        type:Object,
        default:function(){
          return {}
        }
      },
      // 默认选中的节点
      ids:{
        type:Array,
        default:function(){
          return []
        }
      },
      withoutParents:{
        type: Array,
        default: function () {
          return []
        }
      },
      model: {
        type: Object,
        default: function () {
          return {}
        }
      },
      status: {
        type: Number,
        default: 0
      },
      rootStatus: {
        type: Number,
        default: 0
      },
      // 计算哪些节点是父节点，当子节点全部选中时则不排除，然后设置indeterminate样式
      root: {
        type: Array,
        default: function () {
          return []
        }
      },
      options: {
        type: Object,
        default: function () {
          return {}
        }
      }
    },
    data(){
      return{
        // rootItemStatus:0,
        itemStatus: 0,//通过这个参数，来触发watch中的status
      }
    },
    created() {
      // 根据传进来的值，触发选中节点
      if (this.ids.indexOf(this.model.id) !== -1) {
        this.itemStatus = this.itemStatus + 1
        this.$set(this.model, "checked", true)
        this.$emit('onChildChange', true)
      }
      if (this.status > 0) {
        this.addChecked()
        this.itemStatus = this.itemStatus + 1
      }

    },
    mounted() {
    },
    computed:{
    },
    watch: {
      // 父组件改变，则触发子组件跟着改变
      //所以点击全选，全选下面的子元素也跟着触发
      status (val, old) {
        if (val > old) {
          this.addChecked()
          this.itemStatus = this.itemStatus + 1
        } else {
          this.delChecked()
          this.itemStatus = this.itemStatus - 1
        }
       this.setIndeterminateFun();
      }
    },
    methods:{
      // 判断全选是否为横线，子节点全部选中则是打钩
      setIndeterminateFun(){
        if(this.model.child.length>0){
          if(this.root.indexOf(this.model.id) != -1){
            this.deleteRootChecked()
          }
          this.model.indeterminate = false
        }
      },
      toggleChange(){
        // this.model.id就是本次触发的节点ID，查看ids（已经打钩的节点）
        // 是否包含此次选中的节点this.model.id，true则删除，不打钩，false则选中，打钩
        // 然后继续往父组件抛，去触发事件。
        //然后触发status,
        this.setIndeterminateFun();
        if (this.ids.indexOf(this.model.id) !== -1) {
          this.delChecked()
          this.$emit('onChildChange', false)
          this.itemStatus = this.itemStatus - 1
        } else {
          this.addChecked()
          this.$emit('onChildChange', true)
          this.itemStatus = this.itemStatus + 1
        }
      },
      // 选中事件
      addChecked(){
        let _this = this;
        if (this.ids.indexOf(this.model.id) === -1) {
          // 当不包含父节点时，不算进来
          if(!this.options.withParents){
              if(this.model.child && this.model.child.length == 0){
                this.$set(_this.withoutParents, _this.withoutParents.length, _this.model.id);
              }
          }
          this.$set(this.ids, this.ids.length, this.model.id);
          this.$set(this.model, "checked", true)
        }
      },
      //不选中事件
      delChecked () {
        let _this = this;
        let index = this.ids.indexOf(this.model.id)
        if(index != -1){
          this.$delete(this.ids, index);
          this.$set(this.model, "checked", false)
        }
        // 当不包含父节点时，不算进来
        if(!this.options.withParents){
          if(this.model.child && this.model.child.length == 0){
            let key = this.withoutParents.indexOf(this.model.id)
            this.$delete(_this.withoutParents, key);
          }
        }
      },
      // 添加父节点
      setRootChecked(){
        if (this.root.indexOf(this.model.id) === -1) {
          this.$set(this.root, this.root.length, this.model.id)
          // this.rootItemStatus ++
          this.model.indeterminate=true
        }
      },
      // 删除父节点
      deleteRootChecked () {
        let idx = this.root.indexOf(this.model.id)
        if (idx !== -1) {
          this.$delete(this.root, idx)
          // this.rootItemStatus --
          this.model.indeterminate=false
        }
      },
      // 递归组件，往上抛事件
      onChildChange(check){
        if (this.model.child.some((val) => this.root.indexOf(val.id) !== -1)) {
          this.addChecked()
          this.setRootChecked()
          this.$emit('onChildChange', true)
          return
        }
        if(check){
          this.addChecked()
          if (this.model.child.some((val) => this.ids.indexOf(val.id) == -1)) {
            this.setRootChecked()
          } else {
            this.deleteRootChecked()
          }
          this.$emit('onChildChange', true)
        }else{
          // 判断每一层数据的child是否含有ids，如果没有，则删除本身model的id
          if (this.model.child.some((val) => this.ids.indexOf(val.id) !== -1)) {
            this.setRootChecked()
          }else{
            this.delChecked()
            this.deleteRootChecked()
          }
          this.$emit('onChildChange', false)
        }
      }
    },
    components: {}
  }
</script>
<style lang="less" scoped="scoped">
  .tree{
    margin-left: 15px;
    margin-top: 5px;
    .sub_tree_all{
      content: "";display: inline-block; clear: both;
      .single_btn{
        float: left;
      }
    }

  }
</style>
