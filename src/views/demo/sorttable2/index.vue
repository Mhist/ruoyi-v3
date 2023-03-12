<template>
    <div class="app-container">
        <el-table v-loading="loading" :data="userList" @selectionChange="handleSelectionChange">
            <el-table-column type="selection" width="50" align="center" />
            <el-table-column key="userId" label="用户编号" align="center" prop="userId" />
            <el-table-column
                key="userName"
                label="用户名称"
                align="center"
                prop="userName"
                :show-overflow-tooltip="true"
            />
            <el-table-column
                key="nickName"
                label="用户昵称"
                align="center"
                prop="nickName"
                :show-overflow-tooltip="true"
            />
            <el-table-column
                key="phonenumber"
                label="手机号码"
                align="center"
                prop="phonenumber"
                width="120"
            />
        </el-table>
        <pagination
            v-show="total > 0"
            v-model:page="queryParams.pageNum"
            v-model:limit="queryParams.pageSize"
            style="margin-top: 12px"
            :total="total"
            @pagination="getList"
        />

        <el-table
            v-loading="loading"
            style="position: relative; top: 20px"
            :data="userList2"
            @selectionChange="handleSelectionChange"
        >
            <el-table-column type="selection" width="50" align="center" />
            <el-table-column key="userId" label="用户编号" align="center" prop="userId" />
            <el-table-column
                key="userName"
                label="用户名称"
                align="center"
                prop="userName"
                :show-overflow-tooltip="true"
            />
            <el-table-column
                key="nickName"
                label="用户昵称"
                align="center"
                prop="nickName"
                :show-overflow-tooltip="true"
            />
         
            <el-table-column
                key="phonenumber"
                label="手机号码"
                align="center"
                prop="phonenumber"
                width="120"
            />
        </el-table>
        <pagination
            v-show="total2 > 0"
            v-model:page="queryParams.pageNum"
            v-model:limit="queryParams.pageSize"
            :total="total2"
            @pagination="getList"
        />
    </div>
</template>

<script setup name="User" lang="ts">
import { listUser } from '@/api/system/user';
import icon_dot_sort from "assets/images/icon_dot_sort.png";

import {
    getCurrentInstance,
    ComponentInternalInstance,
    ref,
    reactive,
    toRefs,
    watch,
    nextTick,
    onMounted,
} from 'vue';
import { useRouter } from 'vue-router';
import { ElTree } from 'element-plus';
import Sortable from 'sortablejs';
const router = useRouter();
const { proxy } = getCurrentInstance() as ComponentInternalInstance;
const loading = ref(true);
const single = ref(true);
const multiple = ref(true);
const total = ref(0);
const total2 = ref(0);
const userList = ref<Person[]>([]);
const userList2 = ref<Person[]>([]);
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
            {
                pattern: /^1[3|4|5|6|7|8|9][0-9]\d{8}$/,
                message: '请输入正确的手机号码',
                trigger: 'blur',
            },
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
const tableData = ref<any[]>([]);

interface Person {
    userName: string;
    nickName: string;
    phonenumber: number;
    userId:number;
}

const rowDropInit = () => {
    nextTick(() => {
        const tbody1 = document.querySelectorAll('.el-table__body-wrapper tbody')[0] as HTMLElement;
        const tbody2= document.querySelectorAll('.el-table__body-wrapper tbody')[1] as HTMLElement;
        Sortable.create(tbody1, {
            animation: 150, // ms, number 单位：ms，定义排序动画的时间
            handle: '.el-table__row', // 格式为简单css选择器的字符串，使列表单元中符合选择器的元素成为拖动的手柄，只有按住拖动手柄才能使列表单元进行拖动
            delay: 0,
            onEnd: function (e) {
             
                userList.value = [];
              
            },
        });
        userList2.value = [];
        Sortable.create(tbody2, {
            animation: 150, // ms, number 单位：ms，定义排序动画的时间
            handle: '.el-table__row', // 格式为简单css选择器的字符串，使列表单元中符合选择器的元素成为拖动的手柄，只有按住拖动手柄才能使列表单元进行拖动
            delay: 0,
            onEnd: function (e) {
             
                userList2.value = [];
              

            },
        });

    });
};

onMounted(async () => {
    /*------------------
     * 异步加载table数据
     * ------------------
     */
    nextTick(() => {
        rowDropInit();
    });
});

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
