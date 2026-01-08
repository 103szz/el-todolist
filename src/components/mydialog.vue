<template>
  <div class="flex flex-wrap gap-1">
    <el-button type='primary' class="!ml-0" plain @click="dialogFormVisible = true">
      新建待办事项
    </el-button>
  </div>

  <el-dialog v-model="dialogFormVisible" title="新建待办事项" width="500">
    <el-form :model="formData">
      <el-form-item label="事项名称" :label-width="formLabelWidth" required>
        <el-input v-model="formData.name" autocomplete="off" />
      </el-form-item>
      <el-form-item label="事项内容" :label-width="formLabelWidth" required>
        <el-input v-model="formData.content" autocomplete="off" />
      </el-form-item>
      <el-form-item label="创建时间" :label-width="formLabelWidth" required>
         <el-date-picker
        v-model="formData.createTime"
        type="date"
        placeholder="请选择日期"

      />
      </el-form-item>
        <el-form-item label="预计完成时间" :label-width="formLabelWidth" required>
         <el-date-picker
        v-model="formData.competeTime"
        type="date"
        placeholder="请选择日期"

      />
      </el-form-item>
      <el-form-item label="重要性程度" :label-width="formLabelWidth" required>
        <el-select v-model="formData.importance" placeholder="请选择">
          <el-option label="非常重要" value="im2" />
          <el-option label="重要" value="im1" />
          <el-option label="不重要" value="im0" />
        </el-select>
      </el-form-item>
    </el-form>
    <template #footer>
      <div class="dialog-footer">
        <el-button type="primary" @click="handleSubmit">
          添加
        </el-button>
        <el-button @click="dialogFormVisible = false">关闭</el-button>
        
      </div>
    </template>
  </el-dialog>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue'

const dialogFormVisible = ref(false)
const formLabelWidth = '140px'


// 定义 Props 类型
interface Task {
 
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

// 表单数据
const formData = reactive<Task>({
  
  name:'',
  content:'',
  importance:'',
  createTime:new Date().toISOString().split('T')[0],
  competeTime:new Date().toISOString().split('T')[0],
  show: false,
  competed:false
})
// 定义要触发的事件
const emit = defineEmits<{
  (e: 'add-task', taskData: Task): void
}>()
// 提交表单
const handleSubmit = () => {
  // 表单验证
  if (!formData.name.trim()) {
    alert('请输入事件名称')
    return
  }
  formData.competeTime=new Date(formData.competeTime).toISOString().split('T')[0]
  formData.createTime=new Date(formData.createTime).toISOString().split('T')[0]
  
  // 触发事件，将表单数据发送给父组件
  emit('add-task', { ...formData })
  
  // 重置表单
  resetForm()
  
  // 可选：显示成功消息
  alert('任务添加成功！')

}
// 重置表单
const resetForm = () => {
  
  formData.name='',
  formData.content='',
  formData.importance='',
  formData.createTime=new Date().toISOString().split('T')[0],
  formData.competeTime=new Date().toISOString().split('T')[0],
  formData.show=false,
  formData.competed=false
}


</script>