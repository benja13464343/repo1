<script setup>
import Edit from './components/Edit.vue'
import axios from 'axios'
import { onMounted, ref } from 'vue'

// TODO: 列表渲染
// 思路：声明一个效应数据list -> 调用接口获取数据 -> 后端数据赋值到list -> 绑定到table组件

const list = ref([])
const getList = async () => {
  // 接口调用
  const res = await axios.get('/list')
  // 后端数据赋值给list
  list.value = res.data
}

onMounted(() => getList())

// TODO: 删除功能
// 思路： 获取当前行的id -> 通过id调用删除接口与 -> 更新最新的列表

const onDelete = async (id) => {
  console.log(id)
  await axios.delete(`/del/${id}`)
  getList()
}

// TODO: 编辑功能
// 思路：打开弹窗 -> 回填数据 -> 更新数据

// 打卡弹窗
// 回填数据
const editRef = ref(null)
const onEdit = (row) => {
  editRef.value.open(row)
}


</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default="{ row }">
          <el-button type="primary" @click="onEdit(row)">编辑</el-button>
          <el-button type="danger" @click="onDelete(row.id)" link>删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit ref="editRef" @on-update="getList" />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
