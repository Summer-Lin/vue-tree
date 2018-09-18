<template>
  <ul>
    <treenode
      v-for="(model , index) in treeData"
      :model="model"
      :key="index"
      :withoutParents="withoutParents"
      :ids="ids"
      :options="treeOptions"
    >

    </treenode>
  </ul>
</template>
<script>
  /**
   *时间：2018/09/18
   *作者:Summer
   *功能:
   *
   * @参数：
   *
   */
  import tree from './item.vue'
  export default {
    name: 'Tree',
    model: {
      prop: 'ids',
      event: 'change'
    },
    props: {
      treeData:{
        type:Array,
        default:function(){
          return []
        }
      },
      ids: {
        type: Array,
        default: function () {
          return []
        }
      },
      withoutParents:{
        type: Array,
        default: function () {
          return []
        }
      },
      options:{
        type: Object,
        default: function () {
          return {}
        }
      }

    },
    data(){
      return{
        defaultOptions:{
          withParents:true
        },
        treeOptions:{}
      }
    },
    created() {
      this.initOptions();
    },
    mounted() {

    },
    watch:{

    },
    methods:{
      // 初始化默认选项
      initOptions () {
        this.treeOptions = Object.assign({}, this.defaultOptions, this.options)
      }
    },
    components: {
      'treenode':tree
    }
  }
</script>
<style lang="less" scoped="scoped">
  .tree{

  }
</style>
