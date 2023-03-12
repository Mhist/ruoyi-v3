<template lang="">
    <div>
        <div class="red">1</div>
        <code-editor
        ref="codeEditorRef"
        :code="sql"
  
>
</code-editor>
    </div>
</template>
<script lang="ts" setup>
import {ref} from 'vue';
let a = ref(false);
console.log(a,"a");
</script>
<style lang="scss" scoped>
    .red{
        color: red;
    }
</style>
  
  