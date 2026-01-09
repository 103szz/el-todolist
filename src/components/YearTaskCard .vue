<template>
<div>
    <el-row :gutter="20">
        <el-col :span="6" v-for="i in 12">
            <el-card  style="width: 100%" shadow="hover">
                <template #header>
                    <p>{{ i }}月任务进度</p>
                    <el-progress :percentage="calculatePercentage(i-1)"
                    :format="format"
                    :color="customColorMethod"/>
                </template>
                
                <p>总任务数：{{ taskSum[i-1] }}</p>
                <p>已完成任务数：{{ competedSum[i-1] }}</p>
                <p>未完成任务数：{{ taskSum[i-1]-competedSum[i-1] }}</p>
                <!-- <button @click="addcompetedTaskSum(i-1)">add</button> -->
            </el-card>
        </el-col>
    </el-row>

</div>
</template>

<script lang="ts" setup>
import { computed, ref, watch } from 'vue'

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
  // competedSum:number[]
}>()

// const props=defineProps({
//     TaskSum: {
//     type: Array as () => number[],
//     default: () => Array(12).fill(0)  // 必须用函数返回
//   },
//     competedTaskSum: {
//     type: Array as () => number[],
//     default: () => Array(12).fill(0)  // 必须用函数返回
//   },
// })
// const taskSum=ref(Array(12).fill(0))
const competedSum=ref(Array(12).fill(0))
// function getCompetedsum(){
//   for(let i = 0; i < 12; i++){
//     props.tasklist.forEach(task => {
//       let createMonth=Number(task.createTime.split('-')[1])
//       let competeMonth=Number(task.competeTime.split('-')[1])
//       if(createMonth<=(i+1)&&competeMonth>=(i+1)){
//         if(task.competed){
//           competedSum.value[i]+=1
//         }
//       }
//     });
//   }
  
// }

//  function findDeletedIds(oldArray, newArray) {
//     const oldIds = new Set(oldArray.map(item => item.id))
//     const newIds = new Set(newArray.map(item => item.id))
    
//     // 差集：在 oldIds 中但不在 newIds 中的 id
//     const deletedIds = [...oldIds].filter(id => !newIds.has(id))
//     return deletedIds
//   }
// watch(
//   () => props.tasklist,
//   (newList,oldList) => {
//     console.log('待办列表变化:', {
//       新长度: newList.length || 0,
//       旧长度: oldList?.length || 0,
//     })
//     if(newList.length>oldList.length){
//       let createMonth=Number(newList[newList.length-1].createTime.split('-')[1])
//       let competeMonth=Number(newList[newList.length-1].competeTime.split('-')[1])
//       console.log(createMonth+'月开始，'+competeMonth+'月完成');
//       for(let i=createMonth;i<=competeMonth;i++){
//         taskSum.value[i-1]+=1
//       }
//     }
//     else if(newList.length<oldList.length){
//       // 在旧数组中找出不在当前数组中的对象
//      const deletedId = findDeletedIds(oldList,newList)[0]
//       let createMonth=Number(oldList[deletedId].createTime.split('-')[1])
//       let competeMonth=Number(oldList[deletedId].competeTime.split('-')[1])
//       console.log(createMonth+'月开始，'+competeMonth+'月完成');
//       for(let i=createMonth;i<=competeMonth;i++){
//         taskSum.value[i-1]+=1
//     }
//   }
// }
// );
const calculatePercentage =(index) => {
 computed(() => {
    if (props.taskSum[index] === 0) return 0
    return (competedSum[index] / props.taskSum[index]) * 100
  })
}

// const addcompetedTaskSum=(i)=>{
//     if(props.competedTaskSum[i] < props.TaskSum[i])
//     props.competedTaskSum[i]++

// }



const format = (percentage) => (`${Math.round(percentage)}%`)
const customColorMethod = (percentage: number) => {
  if (percentage < 30) {
    return '#909399'
  }
  if (percentage < 85) {
    return '#e6a23c'
  }
  return '#67c23a'
}
</script>