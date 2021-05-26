<template>
  <div class="todo-header">
    <input type="text" placeholder="请输入你的任务名称，按回车键确认" @keyup.enter="add" v-model="inputName" />
  </div>
</template>
<script lang="ts">
import { defineComponent, ref } from "vue";
export default defineComponent({
  name: "Header",
  props: {
    addTodo: {
      type: Function,
      required: true
    }
  },

  setup(props) {
    const inputName = ref("");
    // 按下回车键的回调函数
    const add = () => {
      const text = inputName.value;
      if (!text.trim()) return;
      // 此时有数据，创建一个todo对象
      const todo = {
        id: Date.now(),
        name: text,
        isCompleted: false
      };
      // const handler1 = props.addTodo;
      // if (handler1) {
      //   handler1(todo);
      // }
      props.addTodo(todo);
      inputName.value = "";
    };
    return {
      add,
      inputName
    };
  }
});
</script>
<style scoped>
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
</style>