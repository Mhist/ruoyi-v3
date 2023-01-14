<template>
    <div class="vForm3">
        <v-form-designer ref="vfdRef"></v-form-designer>
        <div>vform3</div>
        <div>vform</div>
    </div>
</template>
    
<script setup lang="ts">
import { ref } from 'vue';
const vfdRef = ref(null);
</script>
    
<style lang="scss" scoped>
body {
    margin: 0;
    /* 如果页面出现垂直滚动条，则加入此行CSS以消除之 */
}

.vForm3{
    margin: 0;
    margin-left:-200px;
    display: flex;
    :deep(.toolbar-header){
        
    }
    :deep(.main-header) {
        display: none;
    }
    :deep(.el-container){
        display: flex;
    }
    :deep(.el-container.main-container .side-panel){
        float: left;
    }
    .form-widget-container {
        flex: 2;
    }
    :deep(.el-aside .el-container .panel-container){
        float: right;

        flex: 1;
    }
}
</style>
