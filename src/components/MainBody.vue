  <template>
      <div class="common-layout">
    <el-container>
      <el-header>
        <h3>待办任务管理器</h3>
        <p>日期：{{ date }}</p>
      </el-header>
      <el-container>
        <el-main>
          <!-- 新建事项 -->
          <mydialog @add-task="handleAddTask" :tasklist="tasklist"/>


          
          <tasks :tasklist="tasklist"
          @update:tasklist="handleListUpdate"/>
        </el-main>
        <el-aside>
          <div type="text">重要提醒</div>
          <aside-timeline :tasklist="tasklist"/>
        </el-aside>
      </el-container>
    </el-container>
  </div>
  </template>

<script lang="ts" setup>
import { ref } from 'vue';
import Tasks from './Tasks.vue';
import AsideTimeline from './asideTimeline.vue';
import Mydialog from './mydialog.vue';
import Mytimeline from './mytimeline.vue';



const date=new Date().toISOString().split('T')[0]

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

// const tasklist=ref<Task[]>([
  // {id:1,name:"示例待办事项",content:"1.....2...3...",importance:"im2",createTime:"2026-01-04",competeTime:"2026-01-04",show: false,competed:false}
  // {id: 1,name:"示例待办事项",content:"1.....2...3...",importance:"im2",createTime:"2026-01-04",competeTime:"2026-01-04",show: false,competed:false}

  // ])
  const createId = (() => {
  let id = 0
  
  return () => {
    const result = ++id
    console.log('createId() 被调用，返回', result)
    return result
  }
})()
 const tasklist=ref<Task[]>([
  {id:createId(),name:"示例待办事项",content:"1.....2...3...",importance:"im2",createTime:"2026-01-04",competeTime:"2026-01-04",show: false,competed:false}
 ])

 const handleAddTask = (newTask: Omit<Task, 'id'>) => {
  const taskWithId: Task = {
    id: createId(),
    ...newTask
  }
  // tasklist.value.push(taskWithId)
  tasklist.value = [...tasklist.value, taskWithId]
  console.log(tasklist.value);
}

const handleListUpdate = (newList) => {
  tasklist.value = newList
}

</script>
