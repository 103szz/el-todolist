<template>
        <div class="grid-content ep-bg-purple" >
    
      <el-row v-for="task in tasklist" :gutter="20">
        <el-card style="width: 800px" shadow="hover">
          <el-row  :gutter="20">
      <el-col :span="16">
        {{task.name}}

      </el-col>
      <el-col :span="8">
         <div class="grid-content ep-bg-purple" >
        <el-button type="success" :icon="Check" circle />
        <el-button type="primary" :icon="Edit" circle ></el-button>
        <el-button type="danger" :icon="Delete" circle></el-button>
    </div>
      </el-col>
      </el-row>
       </el-card>
      </el-row>
   

    </div>
</template>

<script lang="ts" setup>
  import {
  Check,
  Delete,
  Edit,

} from '@element-plus/icons-vue'
import Mycollapse from './mycollapse.vue';
// import type { TabsInstance } from 'element-plus'
// import { ref } from 'vue'
// const tabPosition = ref<TabsInstance['tabPosition']>('left')
const tasklist=[{name:"待办事项1.....",content:"1.....2...3...",importance:"重要",createTime:"",competeTime:""},
{name:"待办事项2.....",content:"1.....2...3...",importance:"非常重要",createTime:"",competeTime:""},
{name:"待办事项3.....",content:"1.....2...3...",importance:"不重要",createTime:"",competeTime:""},
{name:"待办事项4.....",content:"1.....2...3...",importance:"重要",createTime:"",competeTime:""}]

const show=false

</script>