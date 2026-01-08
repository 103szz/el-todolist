<script setup>
const tasklist=[{id:"0",name:"待办事项1.....",content:"1.....2...3...",importance:"重要",createTime:"",competeTime:"2025-12.20",show: false},
{id:"1",name:"待办事项2.....",content:"1.....2...3...",importance:"非常重要",createTime:"",competeTime:"2025-12.25",show: false},
{id:"2",name:"待办事项3.....",content:"1.....2...3...",importance:"不重要",createTime:"",competeTime:"2025-12.27",show: false},
{id:"3",name:"待办事项4.....",content:"1.....2...3...",importance:"重要",createTime:"",competeTime:"2025-12.29",show: false}]
</script>

<template>
  <el-timeline style="max-width: 600px">
    <el-timeline-item center 
    v-for="task in tasklist"
    :timestamp='task.competeTime' placement="top">
      <el-card>
        <p>{{task.content}}</p>
      </el-card>
    </el-timeline-item>
  </el-timeline>
</template>
