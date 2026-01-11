<template>
<div>
      <!-- <mycalendar/> -->
      <!-- <mycalendar-edit/> -->

      <el-calendar v-model="currentDate">
    <!-- 使用已被验证有效的插槽名称：#date-cell -->
    <template #date-cell="{ data }">
      <!-- 动态绑定计算出的样式类 -->
      <div class="custom-date-cell" :class="getDateClass(data)">
        <!-- 显示日期（去掉年份和月份，只留日） -->
        {{ data.day.split('-').pop() }}
        <!-- 可以在这里添加自定义内容，如事件圆点 -->
        <div v-if="myhasEvent(data.day)" class="myevent-indicator"></div>
      </div>
    </template>
  </el-calendar>

</div>

</template>

<script lang="ts" setup>

import { ref } from 'vue'
import Mycalendar from './mycalendar.vue';
import MycalendarEdit from './mycalendarEdit.vue';


// 定义 Props 类型
interface ArrItem {
  id:number,
  name:string,
  content:string,
  importance:string,
  startTime:string,
  competeTime:string,
  show: boolean,
  competed:boolean
}

// 定义 Props
const props = defineProps<{
  tasklist: ArrItem[]
}>()
const currentDate = ref(new Date());

const myhasEvent = (dateString) => {
  const task= props.tasklist.find(user => user.competeTime === dateString)
  if(task)
    return true
  else
    return false
  };

// 3. 核心函数：根据日期决定应用的CSS类名
const getDateClass = (data) => {
  const dateStr = data.day; // 例如 "2025-12-25"
  const classes = [];

  if(myhasEvent(dateStr))
{
  const task= props.tasklist.find(user => user.competeTime === dateStr)
  if(task){
    if(task.competed==true)
      classes.push('competed')
    else
      classes.push(task.importance)
  }
}
  // 返回所有计算出的类名，它们会绑定到 :class
  return classes;
};

</script>

<style scoped>
/* 穿透scoped样式，并确保高特异性以覆盖默认样式 */
:deep(.el-calendar-table td .el-calendar-day) {
  /* 可选：为日期格子设置一个基础高度，使自定义样式更易观察 */
  height: 80px;
  vertical-align: top;
}

/* 自定义单元格基础样式 */
:deep(.custom-date-cell) {
  height: 100%;
  width: 100%;
  padding: 8px 4px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  font-size: 14px;
  border-radius: 4px;
}

/* 周末日期样式 */
:deep(.el-calendar-table td .weekend-day) {
  background-color: #f0f9ff; /* 浅蓝色背景 */
  color: #1890ff;           /* 蓝色文字 */
  font-weight: bold;
}

/* 节假日样式 */
:deep(.el-calendar-table td .holiday) {
  background-color: #fff2e8; /* 浅橙色背景 */
  color: #fa541c;           /* 橙色文字 */
  border: 1px dashed #fa541c;
}

:deep(.el-calendar-table td .im0) {
  background-color: #fff2e8; /* 浅橙色背景 */
  color: #070504;           /* 橙色文字 */
  border: 1px dashed #fa541c;
}
:deep(.el-calendar-table td .im1) {
  background-color: #fe373783; /* 浅橙色背景 */
  color: #000000;           /* 橙色文字 */
  border: 1px dashed #fa541c;
}
:deep(.el-calendar-table td .im2) {
  background-color: #ff5500; /* 浅橙色背景 */
  color: #0c0c0b;           /* 橙色文字 */
  border: 1px dashed #fa541c;
}
:deep(.el-calendar-table td .competed) {
  background-color: #00ff51; /* 浅橙色背景 */
  color: #0c0c0b;           /* 橙色文字 */
  border: 1px dashed #fa541c;
}

/* 任务截止日样式 */
:deep(.el-calendar-table td .deadline) {
  background-color: #f6ffed; /* 浅绿色背景 */
  color: #52c41a;           /* 绿色文字 */
  border: 1px solid #b7eb8f;
}

/* 非本月日期样式（默认已灰色，可强化） */
:deep(.el-calendar-table td .other-month-day .custom-date-cell) {
  color: #c0c4cc;
  opacity: 0.7;
}

/* 事件指示器小圆点 */
.myevent-indicator {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background-color: #ff4d4f; /* 红色圆点 */
  margin-top: 4px;
}
.event-indicator {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background-color: #ff4d4f; /* 红色圆点 */
  margin-top: 4px;
}
</style>