<template>
    <Codemirror
      v-model:value="code"
      :options="cmOptions"
      :border="true"
      @change="onChange"
      @input="onInput"
      @ready="onReady"
      KeepCursorInEnd="true"
    >
    </Codemirror>
  </template>
  <script lang="ts" setup>
  import { ref } from "vue";
  import "codemirror/mode/javascript/javascript.js";
  import 'codemirror/mode/htmlmixed/htmlmixed.js';
  import Codemirror from "codemirror-editor-vue3";
  import type { Editor, EditorConfiguration } from "codemirror";
  
  const code = ref('console.log("name")');
  
  const cmOptions: EditorConfiguration = {
    mode: "text/html",
  };
  
  const onChange = (val: string, cm: Editor) => {
    console.log(val);
    console.log(cm.getValue());
  };
  
  const onInput = (val: string) => {
    console.log(val);
  };
  
  const onReady = (cm: Editor) => {
    console.log(cm.focus());
  };
  </script>
  
  