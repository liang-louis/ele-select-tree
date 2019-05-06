<!-- 树状选择器 el-popover+el-tree+el-input -->
<template>
  <el-popover
    ref="popover"
    placement="bottom-start"
    trigger="manual" v-model="showStatus"
    :disabled="disabled">
    <el-tree
      ref="tree"
      class="select-tree"
      highlight-current
      :style="`min-width: ${treeWidth}`"
      :data="options"
      :props="props"
      :expand-on-click-node="false"
      :filter-node-method="filterNode"
      :default-expand-all="false"
      @node-click="onClickNode"
      :show-checkbox="multiple"
      :node-key="props.value"
      :default-checked-keys="(multiple && Array.isArray(value)) ? value : []"
      @check="clickCheckbox" @mousedown.native="treeMouseDown">
       <span slot-scope="{ node, data }">
         <!--自定义复选框-->
         <span v-if="multiple" class="check_icon">
           <i :class="{'indeterminate':node.indeterminate,'checked':node.checked}"></i>
         </span>
         <span>{{ node.label }}</span>
        </span>
    </el-tree>
    <el-input
      slot="reference"
      ref="input"
      v-model="labelModel"
      :size="size"
      :clearable="clearable" :readonly="disabled"
      :style="`width: ${width}px`"
      @mouseenter.native="inputHovering = true"
      @mouseleave.native="inputHovering = false"
      @clear="inputClear" @input="inputInput" @click.native="clickInput" @blur="inputBlur" @focus="input.focus = true"
      :placeholder="placeholder">
      <i slot="suffix" v-show="!showClose" :class="['el-input__icon el-icon-arrow-down',{ 'rotate': showStatus}]"></i>
    </el-input>
  </el-popover>
</template>

<script>
  export default {
    name: 'SelectTree',
    props: {
      value: { // 接收绑定参数
        type: [String, Number, Array],
      },
      options: { // 选项数据
        type: Array,
        required: true,
      },
      width: String, // 输入框宽度
      placeholder: { // 输入框占位符
        type: String,
        default: '请选择',
      },
      size: {
        type: String,
        default:''
      },
      props: { // 树节点配置选项
        type: Object,
        default() {
          return {
            value: 'value',
            label: 'label',
            children: 'children',
          }
        },
      },
      multiple: { // 是否可多选
        type: Boolean,
        default: false
      },
      filterable: { // 是否可过滤
        type: Boolean,
        default: false
      },
      clearable: { // 是否可清空
        type: Boolean,
        default: true
      },
      disabled: { // 是否禁用
        type: Boolean,
        default: false
      }
    },
    // 设置绑定参数
    model: {
      prop: 'value',
      event: 'selected',
    },
    data() {
      return {
        labelModel:'',
        // 树状菜单显示状态
        showStatus: false,
        // 菜单宽度
        treeWidth: 'auto',
        input: {
          focus: false,//是否获取焦点
          clear: false,//是否点击clear按钮
        },
        inputHovering: false,//input鼠标hover
      };
    },
    created() {
    },
    mounted() {
      // 获取输入框宽度同步至树状菜单宽度
      this.treeWidth = `${(this.width || this.$refs.input.$refs.input.clientWidth) - 24}px`;
    },
    watch: {
      'selectValue'(val,old) {
      }
    },
    computed: {
      // 选择值
      selectValue: {
        get() {
          let label = '';
          const value = this.value;
          if (this.multiple) { //复选
            if (Array.isArray(value) && value.length>0) {
              const labelArr = [];
              value.forEach(value=>{
                labelArr.push(this.queryTree(this.options, value))
              });
              label = labelArr.join(',')
            }
          } else { //单选
            if (value) {
              label = this.queryTree(this.options, value);
            }
          }
          this.labelModel = label;
          return this.value;
        },
        set(val) {
          this.$emit('selected', val);
        }
      },
      showClose() { //是否显示关闭按钮
        let hasValue = this.multiple
          ? Array.isArray(this.value) && this.value.length > 0
          : this.value !== undefined && this.value !== null && this.value !== '';
        return this.clearable && (this.inputHovering || this.showStatus || this.input.focus) && hasValue && !this.disabled;
      }
    },
    methods: {
      // 点击树形控件
      treeMouseDown(e) {
        e.preventDefault();//组织默认行为 使节点可展开 可点击
      },
      // 在输入框输入值
      inputInput(val) {
        if (this.filterable) {
          this.showStatus = true;
          this.$refs.tree.filter(val);
        }
      },
      // 清空选中值
      inputClear() {
        this.input.clear = true;
        if (this.multiple) {
          this.$refs.tree.setCheckedNodes([]);
          this.selectValue = [];
        } else {
          this.selectValue = '';
        }
      },
      // 点击输入框
      clickInput() {
        if (!this.disabled) {
          // 判断是否通过clear触发的点击事件
          if (!this.input.clear) {
            this.showStatus = !this.showStatus;
          } else {
            this.input.clear = false;
            this.blurInput();
          }
        }
      },
      inputBlur() {
        this.showStatus = false;
        this.input.focus = false;
      },
      focusInput() {
        this.$refs.input.focus();
      },
      blurInput() {
        this.$refs.input.blur();
      },

      // 单击节点
      onClickNode(node) {
        if (!this.multiple) {
          // 单选
          this.blurInput();
          this.selectValue = node[this.props.value];
        } else {
          //复选
          const checkedKeys = this.$refs.tree.getCheckedKeys();
          const isCheck = !checkedKeys.includes(node[this.props.value]);
          this.$refs.tree.setChecked(node,isCheck,true);
          this.handleLabelValue();
        }
      },
      // 点击复选框
      clickCheckbox(node, data) {
        this.focusInput();
        this.showStatus = true;
        const values = data.checkedKeys;
        const nodes = data.checkedNodes;
        this.handleLabelValue(nodes,values);
      },
      // 多选 处理显示值，提交值
      handleLabelValue(nodes,datas) {
        nodes = nodes ? nodes : this.$refs.tree.getCheckedNodes();
        datas = datas ? datas : this.$refs.tree.getCheckedKeys();
        this.selectValue = datas;
      },
      // 树节点过滤方法
      filterNode(query, data) {
        if (!query) {
          return true
        } else {
          const labelArray = query.split(',');
          return labelArray.some(value => {
            return value && data[this.props.label].includes(value)
          })
        }
      },
      // 搜索树状数据中的 ID
      queryTree(tree, id) {
        let stark = [];
        stark = stark.concat(tree);
        while (stark.length) {
          const temp = stark.shift();
          if (temp[this.props.children]) {
            stark = stark.concat(temp[this.props.children]);
          }
          if (temp[this.props.value] === id) {
            return temp[this.props.label];
          }
        }
        return '';
      },
    },
  };
</script>

<style scoped lang="scss">
  $color-primary: #2A4FD7;
  .el-tree {
    /deep/ .el-tree-node__content > .el-checkbox { //隐藏原有复选框
      display: none;
    }
    .check_icon {
      white-space: nowrap;
      cursor: pointer;
      outline: none;
      display: inline-block;
      line-height: 1;
      position: relative;
      vertical-align: middle;
      i {
        display: inline-block;
        position: relative;
        border: 1px solid $color-primary;
        border-radius: 2px;
        box-sizing: border-box;
        width: 14px;
        height: 14px;
        background-color: #FFFFFF;
        z-index: 1;
        -webkit-transition: border-color 0.25s cubic-bezier(0.71, -0.46, 0.29, 1.46), background-color 0.25s cubic-bezier(0.71, -0.46, 0.29, 1.46);
        transition: border-color 0.25s cubic-bezier(0.71, -0.46, 0.29, 1.46), background-color 0.25s cubic-bezier(0.71, -0.46, 0.29, 1.46);
        &.checked,&.indeterminate {
          background-color: $color-primary;
          &.indeterminate:before {
            content: '';
            position: absolute;
            display: block;
            background-color: #FFFFFF;
            height: 2px;
            -webkit-transform: scale(0.5);
            transform: scale(0.5);
            left: 0;
            right: 0;
            top: 5px;
          }
          &.checked:after {
            box-sizing: content-box;
            content: "";
            border: 1px solid #fff;
            border-left: 0;
            border-top: 0;
            height: 7px;
            left: 4px;
            position: absolute;
            top: 1px;
            width: 3px;
            transition: transform .15s ease-in .05s;
            transform-origin: center;
            transform: rotate(45deg) scaleY(1);
          }
        }
      }
    }
  }
  /deep/.el-input.el-input--suffix {
    cursor: pointer;
    overflow: hidden;
    &:not(.is-disabled) .el-input__inner{
      cursor: pointer;
    }
    .el-input__suffix i.rotate{
      transform: rotate(180deg);
    }
  }
  .select-tree {
    max-height: 350px;
    overflow-y: scroll;
    /*菜单滚动条*/
    &::-webkit-scrollbar {
      z-index: 11;
      width: 6px;
    }
    &::-webkit-scrollbar-track,&::-webkit-scrollbar-corner {
      background: #fff;
    }
    &::-webkit-scrollbar-thumb {
      border-radius: 5px;
      width: 6px;
      background: #b4bccc;
    }
    &::-webkit-scrollbar-track-piece {
      background: #fff;
      width: 6px;
    }
  }
</style>
