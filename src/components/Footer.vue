<template>
  <div class="todo-footer">
    <label>
      <input type="checkbox" v-model="checkAll" />
    </label>
    <span>全选/全不选</span>
    <button class="btn btn-danger" @click="deleteAllComp">清除已勾选项</button>
    <div>
      <span>
        <span>({{count}}</span>
        /{{todos.length}})
      </span>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, computed } from "vue";
import { Todo } from "../types/todo";
export default defineComponent({
  name: "Footer",
  // props: ["delTodo","todos"],
  props: {
    todos: {
      type: Array as () => Todo[],
      required: true
    },
    selectAll: {
      type: Function,
      required: true
    },
    deleteAllCompleted: {
      type: Function,
      required: true
    }
  },
  setup(props) {
    //const delAll = () => {};
    const count = computed(() => {
      return props.todos.reduce(
        (pre, cur) => pre + (cur.isCompleted ? 1 : 0),
        0
      );
    });

    const checkAll = computed({
      // 此处想到在App父级组件中创建一个操作全选、全部选的函数
      get() {
        return count.value > 0 && props.todos.length === count.value;
      },
      set(val) {
        props.selectAll(val);
      }
    });

    const deleteAllComp = () => {
      if (window.confirm("确定删除所有已勾选项吗？")) {
        props.deleteAllCompleted(props.todos);
      }
    };
    return {
      count,
      checkAll,
      deleteAllComp
    };
  }
});
</script>
<style scoped>
/* footer */
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>