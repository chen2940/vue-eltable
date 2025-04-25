<template>
    <el-table :data="tableData" border style="width: 100%">
      <el-table-column prop="id" label="学号" width="75" />
      <el-table-column prop="name" label="名字" width="75" />
      <el-table-column label="成绩" width="75">
        <template v-slot="scope">
          {{ scope.row.cost >= 60 ? '及格' : '不及格' }}
        </template>
      </el-table-column>
      <el-table-column prop="live" label="宿舍" width="100" />
      <el-table-column label="操作">
        <template v-slot="scope">
          <el-button @click="viewStudent(scope.row),dialogVisibleStu = true" round>查看</el-button>
          <el-button @click="editStudent(scope.row)" type="primary" :icon="Edit" circle />
          <el-button @click="deleteStudent(scope.row.id)" type="danger" :icon="Delete" circle />
        </template>
      </el-table-column>
    </el-table>
  
    <!-- 查看学生信息对话框 -->
    <el-dialog 
    title="学生信息" 
    v-model='dialogVisibleStu'
    :before-close="handleClose"
    >
      <p>学号: {{ currentStudent.id }}</p>
      <p>名字: {{ currentStudent.name }}</p>
      <p>成绩: {{ currentStudent.cost }}</p>
      <p>宿舍: {{ currentStudent.live }}</p>
    </el-dialog>
  
    <!-- 编辑学生信息对话框 -->
    <el-dialog title="编辑成绩" v-model='editDialogVisible'>
      <el-form :model="currentStudent">
        <el-form-item label="成绩">
          <el-input v-model.number="currentStudent.cost" type="number" />
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="editDialogVisible = false">取消</el-button>
        <el-button type="primary" @click="saveChanges">保存</el-button>
      </span>
    </el-dialog>
  </template>
  
  <script lang="ts" setup>
  import { reactive, ref } from 'vue';
  import { ElMessage } from 'element-plus';
  import { ElMessageBox } from 'element-plus'
  import {
    Delete,
    Edit,
  } from '@element-plus/icons-vue'
  
  
  const tableData = reactive([
    { id: '200501', name: '张三', cost: 59, live: '一善书院' },
    { id: '200502', name: '李四', cost: 66, live: '双馨书院' },
    { id: '200503', name: '王五', cost: 36, live: '三创书院' },
    { id: '200504', name: '周六', cost: 99, live: '四实书院' },
    { id: '200505', name: '郑七', cost: 54, live: '八方书院' },
    { id: '200506', name: '王老八', cost: 74, live: '拾德书院' },
  ]);
  
  const dialogVisibleStu = ref(false);
  const editDialogVisible = ref(false);
  const currentStudent = reactive({ id: '', name: '', cost: 0, live: '' });
  
  // 查看学生信息
  const viewStudent = (student) => {
    Object.assign(currentStudent, student);
    dialogVisibleStu.value = true;
  };
  
  // 编辑学生信息
  const editStudent = (student) => {
    Object.assign(currentStudent, student);
    editDialogVisible.value = true;
  };
  
  // 保存修改
  const saveChanges = () => {
    ElMessage.success('成绩已更新');
    editDialogVisible.value = false;
  };
  
  // 删除学生
  const deleteStudent = (id) => {
    const index = tableData.findIndex(student => student.id === id);
    if (index !== -1) {
      if (confirm(`确定要删除学号为 ${id} 的学生吗？`)) {
        tableData.splice(index, 1);
        ElMessage.success('学生已删除');
      }
    }
  };
  const handleClose = (done: () => void) => {
   ElMessageBox.confirm('你确定要返回吗')
    .then(() => {
      done()
    })
    .catch(() => {
      // catch error
    })
  }
  </script>
  
  <style>
  .table {
    text-align: center;
  }
  </style>