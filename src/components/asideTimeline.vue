<script lang='ts' setup>
import { computed } from 'vue';

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

const importantTask = computed(() => {
    return props.tasklist.filter(task => task.importance == 'im2')
  })

</script>
<template>
  <el-timeline style="max-width: 600px">
    <el-timeline-item center 
    v-for="task in importantTask"
    :timestamp='task.competeTime' 
    placement="top">
      <el-card>
        <p>{{task.name}}</p>
        <p>{{task.content}}</p>
      </el-card>
    </el-timeline-item>
  </el-timeline>
</template>

<style scoped>


</style>
