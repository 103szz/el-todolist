<template>

  <el-dialog v-model="dialogVisible" title="修改待办事项" width="500">
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
          修改
        </el-button>
        <el-button @click="close">关闭</el-button>
        
      </div>
    </template>
  </el-dialog>
</template>

<script lang="ts" setup>
import { computed, reactive, ref } from 'vue'



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

// // 定义 Props
const props = defineProps<{
  task:Task
  visible:boolean
}>()
// 定义 Emits
const emit = defineEmits<{
  (e: 'update:visible', visible: boolean): void
}>()
// 使用计算属性实现双向绑定
const dialogVisible = computed({
  get: () => props.visible,
  set: (value) => emit('update:visible', value)
})
const formLabelWidth = '140px'
const close=()=>{
  dialogVisible.value = false
}

// 表单数据
const formData = reactive<Task>({
  id:props.task.id,
  name:props.task.name,
  content:props.task.content,
  importance:props.task.importance,
  createTime:props.task.createTime,
  competeTime:props.task.competeTime,
  show: props.task.show,
  competed:props.task.competed
})
// const formData=JSON.parse(JSON.stringify(props.task))

const handleSubmit = () => {
  if(formData.competeTime!=props.task.competeTime){
    formData.competeTime=new Date(formData.competeTime).toISOString().split('T')[0]
  }
  if(formData.createTime!=props.task.createTime)
    formData.createTime=new Date(formData.createTime).toISOString().split('T')[0]
  for (let key in formData) {
  if (formData.hasOwnProperty(key)) {
    props.task[key] = formData[key];
  }

}
resetForm()

alert('任务修改成功！')
dialogVisible.value = false

}
const resetForm = () => {
  formData.id=props.task.id,
  formData.name=props.task.name,
  formData.content=props.task.content,
  formData.importance=props.task.importance,
  formData.createTime=props.task.createTime,
  formData.competeTime=props.task.competeTime,
  formData.show=props.task.show,
  formData.competed=props.task.competed
}


</script>