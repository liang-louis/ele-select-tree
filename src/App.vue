<template>
  <div id="app">
    <el-form :model="form" inline>
      <el-form-item label="单选下拉">
        <SelectTree v-model="form.value" :options="treeOption"></SelectTree>
      </el-form-item>
      <el-form-item label="多选下拉">
        <SelectTree v-model="form.values" :options="treeOption" multiple></SelectTree>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
  import SelectTree from '@/components/SelectTree.vue'
export default {
  name: 'App',
  components: {SelectTree},
  data() {
    return {
      form: {
        value:'1',
        values:['1']
      },
      treeValue: '1',
      treeOption: [
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
