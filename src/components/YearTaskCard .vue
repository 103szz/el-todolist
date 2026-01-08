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
                
                <p>总任务数：{{ TaskSum[i-1] }}</p>
                <p>已完成任务数：{{ competedTaskSum[i-1] }}</p>
                <p>未完成任务数：{{ TaskSum[i-1]-competedTaskSum[i-1] }}</p>
                <!-- <button @click="addcompetedTaskSum(i-1)">add</button> -->
            </el-card>
        </el-col>
    </el-row>

</div>
</template>

<script lang="ts" setup>
import { computed, ref } from 'vue'

const props=defineProps({
    TaskSum: {
    type: Array as () => number[],
    default: () => Array(12).fill(0)  // 必须用函数返回
  },
    competedTaskSum: {
    type: Array as () => number[],
    default: () => Array(12).fill(0)  // 必须用函数返回
  },
})

const calculatePercentage =(index) => {
 computed(() => {
    if (props.TaskSum[index] === 0) return 0
    return (props.competedTaskSum[index] / props.TaskSum[index]) * 100
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