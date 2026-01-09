<template>
  <div>
  <el-tabs :tab-position="tabPosition"  class="demo-tabs">
    <el-tab-pane v-for="tab in tabs" :label="tab.name">
      <today-task-card :tasklist="todayTasks" v-if="tab.id===0"
      @update:tasklist="handleListUpdate"
      @delete:tasklist="handleListDelete"
      @update:competed-sum="handleCompetedsumUpdate"/>
      <week-task-card :tasklist="sortByDate()" v-if="tab.id===1"
      @delete:tasklist="handleListDelete"
      @update:competed-sum="handleCompetedsumUpdate"/>
      <month-task-card :tasklist="tasklist" v-if="tab.id===2"/>
      <year-task-card 
      :competedSum="competedSum"
      :tasklist="tasklist"
      :taskSum="taskSum"
      v-if="tab.id===3"/>
    </el-tab-pane>
  </el-tabs>
    
  </div>

</template>

<style scoped>


</style>

<script lang="ts" setup>
import type { TabsInstance } from 'element-plus'
import { computed, ref } from 'vue'
import TodayTaskCard from './TodayTaskCard.vue';
import Mycollapse from './mycollapse.vue';
import WeekTaskCard from './WeekTaskCard.vue';
import MonthTaskCard from './MonthTaskCard.vue';
import YearTaskCard from './YearTaskCard .vue';
const tabPosition = ref<TabsInstance['tabPosition']>('left')
const tabs=[{name:"今日待办事项",id:0},
{name:"全部待办事项",id:1},
{name:"本月完成进度",id:2},
{name:"年度完成情况",id:3}]

// 定义 Props 类型
interface ArrItem {
  id:number,
  name:string,
  content:string,
  importance:string,
  createTime:string,
  competeTime:string,
  show: boolean,
  competed:boolean
}

// 定义 Props
const props = defineProps<{
  tasklist: ArrItem[],
  taskSum:number[],
  competedSum:number[]
}>()

// const props=defineProps({
//   tasklist: {
//     type:Array as () => ArrItem[]
//   },
//     TaskSum: {
//     type: Array as () => number[],
//     default: () => Array(12).fill(0)  // 必须用函数返回
//   },
//     competedTaskSum: {
//     type: Array as () => number[],
//     default: () => Array(12).fill(0)  // 必须用函数返回
//   },
// })

const today=new Date().toISOString().split('T')[0]
const todayTasks = computed(() => {
    return props.tasklist.filter(task => task.competeTime >= today && task.createTime <= today)
  })
console.log(todayTasks);

function sortByDate(dateField = 'competeTime') {
  // 创建原数组的浅拷贝
  const sortedTasklist = [...props.tasklist]
  
  // 按日期字符串排序（YYYY-MM-DD格式可以直接比较）
  sortedTasklist.sort((a, b) => {
    // 如果日期为空或无效，放在最后
    if (!a[dateField]) return 1
    if (!b[dateField]) return -1
    
    return a[dateField].localeCompare(b[dateField])
  })
  
  return sortedTasklist
}

const emit = defineEmits(['update:tasklist','delete:tasklist','update:competedSum'])
const handleListUpdate = (newList) => {
  emit('update:tasklist', newList)
}
const handleListDelete = (newList,delteId) => {
  emit('delete:tasklist', newList,delteId)
}
const handleCompetedsumUpdate= (task) => {
  emit('update:competedSum', task)
}

// const tasksum=ref(Array(12).fill(0))

// const competedSum=ref(Array(12).fill(0))


// function getTasksum(){
//   for(let i = 0; i < 12; i++){
//     props.tasklist.forEach(task => {
//       let createMonth=Number(task.createTime.split('-')[1])
//       let competeMonth=Number(task.competeTime.split('-')[1])
//       if(createMonth<=(i+1)&&competeMonth>=(i+1)){
//         tasksum.value[i]+=1
//         if(task.competed){
//           competedSum.value[i]+=1
//         }
//       }
//     });
//   }
// }
 

</script>

