<template>
  <div id="app">
    <el-form :model="form" inline>
      <el-form-item label="popper单选">
        <SelectTree v-model="form.value" :options="treeOption"></SelectTree>
      </el-form-item>
      <el-form-item label="popper多选">
        <SelectTree v-model="form.values" :options="treeOption" multiple></SelectTree>
      </el-form-item>
      <el-form-item label="select单选">
        <TreeSelect v-model="form.val" :options="treeOption" filterable></TreeSelect>
      </el-form-item>
      <el-form-item label="select多选">
        <TreeSelect v-model="form.vals" :options="treeOption" multiple filterable></TreeSelect>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
  import SelectTree from '@/components/SelectTree.vue'
  import TreeSelect from '@/components/TreeSelect.vue'
export default {
  name: 'App',
  components: {SelectTree,TreeSelect},
  data() {
    return {
      form: {
        value:'1',
        values:['1'],
        val:'-',
        vals:['-'],
      },
      treeValue: '1',
      treeOption: [ //测试数据
        {
          label: '一',
          value:'1',
          children: [
            {
              label: '一-1',
              value:'11',
            }
          ]
        },
        {
          label: 'er',
          value:'2',
          children: [
            {
              label: 'er-1',
              value:'21',
            }
          ]
        },{
          label: '略略略',
          value:'略略略',
          children: [
            {
              label: '略略略-1',
              value:'略略略---',
            },{
              label: '略略略-2',
              value:'略略略2---',
            },{
              label: '略-1',
              value:'-',
            },{
              label: '略-2',
              value:'2-',
            },{
              label: '1',
              value:'--',
            },{
              label: '2',
              value:'2---',
            },{
              label: '略略1',
              value:'略略--',
            },{
              label: '略略2',
              value:'略略2--',
            },{
              label: '略略略1',
              value:'略略略--',
            },{
              label: '略略略2',
              value:'略略略2--',
            },{
              label: '略-1',
              value:'略---',
            },{
              label: '略-2',
              value:'略2---',
            },{
              label: '略略-1',
              value:'略略---',
            },{
              label: '略略-2',
              value:'略略2---',
            }
          ]
        }
      ]
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
