<template>
  <el-dialog
      title="新增待办计划"
      :modelValue="visible"
      width="600px"
      @close="handleClose"
  >
    <el-form
        style="width: 430px"
        :model="form"
        :rules="rules"
        label-width="120px"
        ref="formRef"
    >
      <el-form-item label="计划名称" prop="title">
        <el-input
            v-model="form.title"
            placeholder="请输入待办计划名称"
        ></el-input>
      </el-form-item>
      <el-form-item label="计划完成时间" prop="time">
        <el-date-picker value-format="YYYY/MM/DD" style="width: 100%" v-model="form.time" type="date" placeholder="请选择计划完成时间">
        </el-date-picker>
      </el-form-item>
      <el-form-item label="计划详细描述" prop="desc">
        <el-input
            type="textarea"
            :rows="6"
            v-model="form.desc"
            placeholder="请输入详细待办计划"
        ></el-input>
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="visible = false">取 消</el-button>
        <el-button type="primary" @click="handleConfirm">确 定</el-button>
      </span>
    </template>
  </el-dialog>
</template>

<script>
  import { reactive, toRefs, ref, inject } from "vue";
  export default {
    name: "add-todo",
    props: {
      visible: {
        type: Boolean,
        default: false,
      },
    },
    setup(props, { emit }) {
      const formRef = ref(null)
      const addTodo = inject('addTodo')
      const state = reactive({
        form: {
          title: "",
          desc: "",
          time: "",
          status: false
        },
        rules: {
          title: [
            { required: true, message: '请输入待办计划名称', trigger: ['blur']}
          ],
          time: [
            { required: true, message: '请选择待办计划时间', trigger: ['change']}
          ],
          desc: [
            { required: true, message: '请输入详细待办计划', trigger: ['blur']}
          ]
        },
      });
      const handleClose = () => {
        emit("update:visible", false);
        formRef.value.clearValidate()
        formRef.value.resetFields()
      };
      const handleConfirm = () => {
        formRef.value.validate(valid => {
          if (valid) {
            addTodo(JSON.parse(JSON.stringify(state.form)))
            handleClose()
          }
        })
      }
      return {
        formRef,
        ...toRefs(state),
        handleClose,
        handleConfirm
      };
    },
  };
</script>

<style scoped>

</style>