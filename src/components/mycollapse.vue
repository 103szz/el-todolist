<template>
  <div class="demo-collapse">
    <el-collapse v-model="activeNames" @change="handleChange">
      <el-collapse-item v-for="task in tasklist" :title="task.name" :name="task.id" :icon="CaretRight">
        <el-row :gutter="20">
            <el-col :span="16"/>
            
            <el-col :span="8">
            <div class="grid-content ep-bg-purple" >
                <el-button type="success" :icon="Check" circle />
                <el-button type="primary" :icon="Edit" circle ></el-button>
                <el-button type="danger" :icon="Delete" circle></el-button>
            </div>
            </el-col>
        </el-row>
        <p>预计完成时间：{{ task.competeTime }}</p>
        <p>重要性：{{ task.importance }}</p>
        <p>创建时间：{{ task.createTime }}</p>
        <p>主要内容：{{ task.content }}</p>
      </el-collapse-item>
    </el-collapse>
  </div>
</template>

<script lang="ts" setup>
  import {
  Check,
  Delete,
  Edit,
} from '@element-plus/icons-vue'
import { ref } from 'vue'
import { CaretRight } from '@element-plus/icons-vue'

import type { CollapseModelValue } from 'element-plus'

const activeNames = ref(['1'])
const handleChange = (val: CollapseModelValue) => {
  console.log(val)
}

const tasklist=[{id:'1',name:"待办事项1.....",content:"1.....2...3...",importance:"重要",createTime:"",competeTime:""},
{id:'2',name:"待办事项2.....",content:"1.....2...3...",importance:"非常重要",createTime:"",competeTime:""},
{id:'3',name:"待办事项3.....",content:"1.....2...3...",importance:"不重要",createTime:"",competeTime:""},
{id:'4',name:"待办事项4.....",content:"1.....2...3...",importance:"重要",createTime:"",competeTime:""}]
</script>

<style scoped>
.icon-ele {
  margin: 0 8px 0 auto;
  color: #409eff;
}
</style>