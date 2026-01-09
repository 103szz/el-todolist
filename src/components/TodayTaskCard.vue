<template>
  <div>   
<el-card v-for="task in tasklist" style="max-width: 800px">
    <template #header>
      <div class="card-header" >
        <el-row :gutter="20" >
            <el-col :span="16">
            <div class="grid-content ep-bg-purple" >
            <span>{{task.name}}预计完成时间：{{ task.competeTime }}</span>
            <!-- <p>{{task.show}}</p> -->
            </div>
            </el-col>
            <el-col :span="8">
            <div class="grid-content ep-bg-purple" >
                <el-button type="success" :icon="Check" :disabled="task.competed" @click="changeState(task)" circle />
                <el-button type="primary" :icon="Edit" circle @click="editDialogFormVisible = true"></el-button>
                <el-button type="danger" :icon="Delete" circle @click="deleteTask(task)"></el-button>
                <el-button @click="changeShow(task)" :icon="arrowStyle(task.show)" circle></el-button>
                <edit-dialog  v-model:visible="editDialogFormVisible" :task="task"/>
            </div>
            </el-col>
        </el-row>
      </div>
    </template>
    <div v-show="task.show" style="max-height: 1000px">
      <!-- <p>{{task.show }}</p> -->
      <p>重要性：{{ getImportance(task.importance) }}</p>
      <p>创建时间：{{ task.createTime }}</p>
      <p>主要内容：{{ task.content }}</p>
    </div>
    
    
  </el-card>



  <!-- <mycollapse/> -->
    
  </div>

</template>

<script lang="ts" setup>
  import {
  Check,
  Delete,
  Edit,
  ArrowRight,
  ArrowDown

} from '@element-plus/icons-vue'
// import type { TabsInstance } from 'element-plus'
import { ref } from 'vue'
import EditDialog from './editDialog.vue';

// 定义 Props 类型
interface Task {
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
  tasklist: Task[]
}>()

function arrowStyle(show){
  if(show){
    return ArrowDown
  }else{
    return ArrowRight
  }
    
  
}

function changeShow(task){
  if (task) {
    task.show = !task.show

    console.log(`项目 ${task.id} 状态改为:`, task.show)
  }
}

function changeState(task){
  if(!task.competed){
    task.competed=true
    console.log(`项目 ${task.id} 已完成`)
  }
}

const isImportance=["不重要","一般重要","很重要"]
function getImportance(im){
  const im_id=im[im.length-1]  
  return isImportance[im_id]
  
}

const editDialogFormVisible=ref(false)
const emit = defineEmits(['update:tasklist','delete:tasklist'])

const deleteTask=(task)=>{
  const newtasklist = props.tasklist.filter(item => item.id !== task.id);
  emit('delete:tasklist', newtasklist,task.id)
  }

</script>



