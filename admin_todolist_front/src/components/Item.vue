<template>
  <div class="todo-item">
    <div class="titme-box">
      <el-checkbox v-model="info.status" @click="change"></el-checkbox>
      <h3 :class="info.status ? 'success' : ''">{{ info.title }}</h3>
    </div>
    <div class="del">
      <p class="time" :class="info.status ? 'success' : ''">{{ info.time }}</p>
      <el-button
          icon="Delete"
          circle
          size="small"
          @click="handleDelTodo"
      ></el-button>
    </div>
  </div>
</template>

<script>
  import { inject } from "vue";
  export default {
    name: "todo-item",
    props: {
      info: {
        type: Object,
        default: () => ({}),
      },
    },
    setup(props) {
      const delTodo = inject("delTodo");
      const handleDelTodo = () => {
        delTodo(props.info.title);
      };
      return {
        handleDelTodo,
      };
    },
  };
</script>

<style scoped>
  .todo-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 1px solid rgb(229, 226, 226);
    height: 45px;
    line-height: 45px;
  }
  .success {
    text-decoration: line-through;
  }
  .titme-box {
    display: flex;
    align-items: center;
  }
  h3 {
    padding-left: 12px;
    font-size: 16px;
  }

  .del {
    display: flex;
    align-items: center;
  }
  .time {
    width: 100px;
    font-size: 14px;
  }

</style>