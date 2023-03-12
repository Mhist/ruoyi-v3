<template>
    <div class="app-container">

    <el-table v-loading="loading" :data="userList" @selectionChange="handleSelectionChange">
        <el-table-column type="selection" width="50" align="center" />
        <el-table-column key="userId" label="用户编号" align="center" prop="userId" />
        <el-table-column
 key="userName" label="用户名称" align="center" prop="userName"
            :show-overflow-tooltip="true" />
        <el-table-column
 key="nickName" label="用户昵称" align="center" prop="nickName"
            :show-overflow-tooltip="true" />
        <el-table-column
 key="deptName" label="部门" align="center" prop="dept.deptName"
            :show-overflow-tooltip="true" />
        <el-table-column
 key="phonenumber" label="手机号码" align="center" prop="phonenumber"
            width="120" />

    </el-table>
    <pagination
    style="margin-top: 12px;"
v-show="total > 0" v-model:page="queryParams.pageNum" v-model:limit="queryParams.pageSize"
        :total="total" @pagination="getList" />

        <el-table style="position:relative;top:20px" v-loading="loading" :data="userList2" @selectionChange="handleSelectionChange">
        <el-table-column type="selection" width="50" align="center" />
        <el-table-column key="userId" label="用户编号" align="center" prop="userId" />
        <el-table-column
 key="userName" label="用户名称" align="center" prop="userName"
            :show-overflow-tooltip="true" />
        <el-table-column
 key="nickName" label="用户昵称" align="center" prop="nickName"
            :show-overflow-tooltip="true" />
        <el-table-column
 key="deptName" label="部门" align="center" prop="dept.deptName"
            :show-overflow-tooltip="true" />
        <el-table-column
 key="phonenumber" label="手机号码" align="center" prop="phonenumber"
            width="120" />

    </el-table>
    <pagination
v-show="total2 > 0" v-model:page="queryParams.pageNum" v-model:limit="queryParams.pageSize"
        :total="total2" @pagination="getList" />
    </div>
</template>

<script setup name="User" lang="ts">
import {
    listUser,
} from '@/api/system/user';

import { getCurrentInstance, ComponentInternalInstance, ref, reactive, toRefs, watch } from 'vue';
import { useRouter } from 'vue-router';
import { ElTree } from 'element-plus';

const router = useRouter();
const { proxy } = getCurrentInstance() as ComponentInternalInstance;


const loading = ref(true);
const single = ref(true);
const multiple = ref(true);
const total = ref(0);
const total2 = ref(0);
const userList = ref<any[]>([]);
const userList2 = ref<any[]>([]);
const dateRange = ref<any[]>([]);
const ids = ref<any[]>([]);
const ids2 = ref<any[]>([]);
const data = reactive<{
    form: any;
    queryParams: any;
    rules: any;
}>({
    form: {},
    queryParams: {
        pageNum: 1,
        pageSize: 10,
        userName: undefined,
        phonenumber: undefined,
        status: undefined,
        deptId: undefined,
    },
    rules: {
        userName: [
            { required: true, message: '用户名称不能为空', trigger: 'blur' },
            { min: 2, max: 20, message: '用户名称长度必须介于 2 和 20 之间', trigger: 'blur' },
        ],
        nickName: [{ required: true, message: '用户昵称不能为空', trigger: 'blur' }],
        password: [
            { required: true, message: '用户密码不能为空', trigger: 'blur' },
            { min: 5, max: 20, message: '用户密码长度必须介于 5 和 20 之间', trigger: 'blur' },
        ],
        email: [{ type: 'email', message: '请输入正确的邮箱地址', trigger: ['blur', 'change'] }],
        phonenumber: [
            { pattern: /^1[3|4|5|6|7|8|9][0-9]\d{8}$/, message: '请输入正确的手机号码', trigger: 'blur' },
        ],
    },
});

const { queryParams, form, rules } = toRefs(data);

/** 查询用户列表 */
function getList() {
    loading.value = true;
    listUser(proxy!.addDateRange(queryParams.value, dateRange.value)).then((res: any) => {
        loading.value = false;
        userList.value = res.rows;
        userList2.value = res.rows;
        total.value = res.total;
        total2.value = res.total;
    });
}


/** 选择条数  */
function handleSelectionChange(selection: any[]) {
    ids.value = selection.map(item => item.userId);
    single.value = selection.length !== 1;
    multiple.value = !selection.length;
}


getList();
</script>


<style lang="scss" scoped>
.hiddenSearch {
    // display: flex;
    // justify-content: flex-end;
   position: relative;
   right: 200px;
}


</style>