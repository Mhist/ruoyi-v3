<template>
  <div class="wrap-container">
    <div class="title">   
      <a href="https://github.com/zyqwst/json-schema-editor-vue" target="_blank">json-schema-editor-vue</a> Preview
    </div>
    <div class="desc">
      <div>A json-schema editor of high efficient and easy-to-use, base on Vue. 
        <a @click="visible = true">导入JSON</a>
      </div>
    </div>
    <div class="container">
      <Codemirror
      class="left-codemirror"
      v-model:value="jsonStr"
      :options="cmOptions"
      :border="true"
      KeepCursorInEnd="true"
    >
    </Codemirror> 
    <div class="schema-box">
      <json-schema-editor class="schema" :value="tree" disabledType lang="zh_CN" custom/>
    </div>
    </div>
    <el-dialog
  title="导入json"
  v-model="visible"
  width="100%"
  draggable
>
<div class="">
  <!-- <Codemirror
      
      v-model:value="importJson"
      :options="cmOptions"
      :border="true"
      KeepCursorInEnd="true"
    >
    </Codemirror> -->
    <vue-json-pretty :data="JSON.parse(importJson)" :showLength="true" :showSelectController="true" :showLineNumber="false" :deep="3" :editable="false" :showDoubleQuotes="true" :showIcon="true"/> 
    <json-viewer :value="JSON.parse(importJson)" copyable boxed sort />
    </div>
  <template #footer>
    <span class="dialog-footer">
      <el-button @click="visible = false">取 消</el-button>
      <el-button type="primary" @click="handleImportJson()">确 定</el-button>
    </span>
  </template>
</el-dialog>
    
  </div>
</template>

<script lang="ts" setup>
  import {ref,computed,reactive} from 'vue';
  import GenerateSchema from 'generate-schema';
  import "codemirror/mode/javascript/javascript.js";
  import 'codemirror/mode/htmlmixed/htmlmixed.js';
  import Codemirror from "codemirror-editor-vue3";
  import type { Editor, EditorConfiguration } from "codemirror";
  import VueJsonPretty from 'vue-json-pretty';
import 'vue-json-pretty/lib/styles.css';
import "vue3-json-viewer/dist/index.css";
import { assertJSXMemberExpression } from '@babel/types';
  const cmOptions: EditorConfiguration = {
    mode: "application/json",
  };
let obj = {
  name: "qiu",//字符串
  age: 18,//数组
  isMan:false,//布尔值
  date:new Date(),
  fn:()=>{},
  arr:[1,2,5]
};
const jsonData = reactive(obj);
  const visible = ref(false);

  let jsonStr = computed({
  // getter
  get: function () {
        return JSON.stringify(tree.value, null, 2)
      },
  // setter
  set: function (newVal) {
        tree.value = JSON.parse(newVal)
      }
})
let   importJson = jsonStr;


  const tree=ref();
    // 导入json
  tree.value = {
root: {
    type: "object",
    "title": "条件",
    "properties": {
      "name": {
        "type": "string",
        "title": "名称",
        "maxLength": 10,
        "minLength": 2
      },
      "appId": {
        "type": "integer",
        "title": "应用ID"
      },
      "credate": {
        "type": "string",
        "title": "创建日期",
        "format": "date"
      }
    },
    "required": [
      "name",
      "appId",
      "credate"
    ]
  }
}
const jsonViewerData = {
  name:"江亚东",
  project:"软件生产线平台",
  inner:{
    position:"武汉省湖北市",
    work:"fronted"
  }
};
function handleImportJson () {
      const  t = GenerateSchema.json(JSON.parse(importJson.value))
      delete t.$schema
      tree.value.root = t 
       visible.value = false
    };
</script>

<style lang="scss" scoped>
.wrap-container{
 overflow-y: hidden;
}
.title{
  text-align: center;
  font-size: 40px;
  font-weight: bold;
  height:100px;
  line-height: 100px;
}
.desc{
  padding:20px;
  width:80vw;
  min-width:800px;
  margin:auto;
  padding: 0 3em;
  font-size: 1.2em;
}
.container{
  display: flex;
  padding:20px;
  min-width:800px;
  justify-content:center;
  height: calc(100vh - 150px);
  margin:auto;
  .codemirror-container.width-auto {
    width: 50%;
}
}

.container2{
  display: flex;
  padding:20px;
  width:90vw;
  min-width:800px;
  justify-content:center;
  height: calc(100vh - 150px);
  margin:auto;
  .codemirror-container.width-auto {
    width: 100%;
}
}

.schema-box{
  height: calc(100vh - 150px);
  overflow: auto;
}
 
</style>