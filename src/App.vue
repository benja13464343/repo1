<script setup>
import Edit from './components/Edit.vue'
import axios from 'axios'
import { onMounted ,ref } from 'vue'

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


// TODO: 编辑功能

</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default>
          <el-button type="primary" link>编辑</el-button>
          <el-button type="danger" link>删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
