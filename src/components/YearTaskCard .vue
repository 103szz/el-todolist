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
  competedSum:number[]
}>()

const calculatePercentage =(index) => {
    if (props.taskSum[index] === 0) return 0
    return (props.competedSum[index] / props.taskSum[index]) * 100
}


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