# ele-select-tree

> 基于vue+element UI的下拉树

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```
尝试了两种实现方式，推荐第二种

## 属性(select-tree) 基于el-popper+el-tree+el-input实现
参数|说明|类型|可选值|默认值
:-|:-|:-|:-|:-
v-model|绑定值|String/Number/Array|—|—
width|输入框宽度，如不指定则自适应容器|String|—|—
options|选项数据|Array|—|必填
placeholder|输入框占位符|String|—|请选择
props|树节点配置选项，具体看下表|Object|—|—
multiple|是否可多选|Boolean|—|false
filterable|是否可过滤|Boolean|—|false
clearable|是否可清空|Boolean|—|true
disabled|是否禁用|Boolean|—|false
size|输入框尺寸|String|medium/small/mini|—

## 属性(tree-select) 基于el-select+el-tree实现（推荐）
参数|说明|类型|可选值|默认值
:-|:-|:-|:-|:-
v-model|绑定值|String/Number/Array|—|—
options|选项数据|Array|—|[]
placeholder|输入框占位符|String|—|请选择
props|树节点配置选项，具体看下表|Object|—|—
multiple|是否可多选|Boolean|—|false
filterable|是否可过滤|Boolean|—|false
clearable|是否可清空|Boolean|—|true
disabled|是否禁用|Boolean|—|false
accordion|是否每次只展开一个同级树节点|Boolean|—|false
size|输入框尺寸|String|medium/small/mini|—
checkStrictly|父子节点选择状态是否不互相关联|Boolean|—|false

## props
参数|说明|类型|可选值|默认值
:-|:-|:-|:-|:-
value|指定节点实际值为节点对象的某个属性值|String|—|'value'
label|指定节点显示值为节点对象的某个属性值|String|—|'label'
children|指定子树为节点对象的某个属性值|String|—|'children'

[Demo](https://shenwangchuan.github.io/ele-select-tree/dist/)
