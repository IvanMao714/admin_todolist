<template>
  <div class="todo-list">
    <el-card class="box-card">
      <template #header>
        <div class="card-header">
          <span>工作计划</span>
          <el-button
              class="button"
              type="primary"
              icon= "Edit"
              circle
              @click="handleClickTodo"
          ></el-button>
        </div>
      </template>
      <template v-if="list.length > 0">
        <todo-item
            v-for="(val, index) in list"
            :key="index"
            :info="val"
        ></todo-item>
      </template>
      <el-empty v-else description="还没有待办的事项~"></el-empty>
    </el-card>
    <add-action v-model:visible="visible"></add-action>
  </div>
</template>

<script>
  import AddAction from "./AddTodo.vue";
  import TodoItem from "./Item.vue";
  import { reactive, toRefs, provide } from "vue";

  export default {
    name: "todo-list",
    components: { AddAction, TodoItem },
    setup() {
      const state = reactive({
        visible: false,
        list: [
          {
            title: "1.学习vue3.0",
            time: "2021-08-20",
            desc: "1.ppppppppppppp",
            status: false,
          },
        ],
      });
      const addTodo = (data) => {
        state.list.push(data);
      };
      const delTodo = (title) => {
        state.list = state.list.filter((val) => val.title !== title);
      };
      const handleClickTodo = () => {
        state.visible = true;
      };
      provide("addTodo", addTodo);
      provide("delTodo", delTodo);
      return {
        handleClickTodo,
        ...toRefs(state),
      };
    },
  };
</script>

<style scoped>
  .todo-list {
    padding: 100px;
  }
  .card-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .text {
    font-size: 14px;
  }
  .item {
    margin-bottom: 18px;
  }
  .box-card {
    width: 480px;
  }
</style>