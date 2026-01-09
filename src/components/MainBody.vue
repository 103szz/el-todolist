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
          @delete:tasklist="DeleteListUpdate"
          :taskSum="taskSum"/>
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


 const taskSum=ref(Array(12).fill(0))
const competedSum=ref(Array(12).fill(0))

 const handleAddTask = (newTask: Omit<Task, 'id'>) => {
  const taskWithId: Task = {
    id: createId(),
    ...newTask
  }
  // tasklist.value.push(taskWithId)
  tasklist.value = [...tasklist.value, taskWithId]
  console.log(tasklist.value);

  // 新建任务时增加任务数量
  let createMonth=Number(taskWithId.createTime.split('-')[1])
      let competeMonth=Number(taskWithId.competeTime.split('-')[1])
      console.log(createMonth+'月开始，'+competeMonth+'月完成');
      for(let i=createMonth;i<=competeMonth;i++){
        taskSum.value[i-1]+=1
      }
}

const handleListUpdate = (newList) => {
  tasklist.value = newList

}
const DeleteListUpdate = (newList,deleteId) => {
  const deleteTask=tasklist.value.filter(item=>item.id==deleteId)[0]
  
  let createMonth=Number(deleteTask.createTime.split('-')[1])
  let competeMonth=Number(deleteTask.competeTime.split('-')[1])
  console.log(createMonth+'月开始，'+competeMonth+'月完成');
  for(let i=createMonth;i<=competeMonth;i++){
    taskSum.value[i-1]+=1
  tasklist.value = newList
  }

}
// const tasksum=ref(Array(12).fill(0))
// const competedSum=ref(Array(12).fill(0))
// function getTasksum(){
//   for(let i = 0; i < 12; i++){
//     tasklist.value.forEach(task => {
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
// getTasksum()
</script>
