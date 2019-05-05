# ele-select-tree
<<<<<<< HEAD

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

## 属性
参数|说明|类型|可选值|默认值
:-|:-|:-|:-|:-
v-model|绑定值|String/Number/Array|—|—
width|输入框宽度，如不指定则自适应容器|String|—|—
options|选项数据|Array|—|必填
placeholder|输入框占位符|String|—|"请选择"
props|树节点配置选项，具体看下表|Object|—|—
multiple|是否可多选|Boolean|true/false|false
filterable|是否可过滤|Boolean|true/false|false
clearable|是否可清空|Boolean|true/false|true
disabled|是否禁用|Boolean|true/false|false

## props
参数|说明|类型|可选值|默认值
:-|:-|:-|:-|:-
value|指定节点实际值为节点对象的某个属性值|String|—|'value'
label|指定节点显示值为节点对象的某个属性值|String|—|'label'
children|指定子树为节点对象的某个属性值|String|—|'children'
