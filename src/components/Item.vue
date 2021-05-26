<template>
  <li
    @mouseenter="mouseHandler(true)"
    @mouseleave="mouseHandler(false)"
    :style="{backgroundColor: bgColor,color: fontColor}"
  >
    <label>
      <input type="checkbox" v-model="isComp" />
      <span>{{todo.name}}</span>
    </label>
    <button class="btn btn-danger" v-if="isShow" @click="delItem">删除</button>
  </li>
</template>
<script lang="ts">
import { defineComponent, ref, computed } from "vue";
import { Todo } from "../types/todo";
export default defineComponent({
  name: "Item",
  props: {
    todo: {
      type: Object as () => Todo,
      required: true
    },
    delTodo: {
      type: Function,
      required: true
    },
    index: {
      type: Number,
      required: true
    },
    updateComp: {
      type: Function,
      required: true
    }
  },
  // data: function() {
  //   return {
  //     localTodo: this.todo
  //   };
  // },
  setup(props) {
    const bgColor = ref("while");
    const fontColor = ref("black");
    const isShow = ref(false);
    const mouseHandler = (flag: boolean) => {
      if (flag) {
        // 鼠标进入
        bgColor.value = "pink";
        fontColor.value = "green";
        isShow.value = true;
      } else {
        // 鼠标离开
        bgColor.value = "white";
        fontColor.value = "black";
        isShow.value = false;
      }
    };

    const isComp = computed({
      get() {
        return props.todo.isCompleted;
      },
      set(val: boolean) {
        props.updateComp(props.todo, val);
      }
    });
    const delItem = () => {
      if (window.confirm("确认要删除吗？")) {
        props.delTodo(props.index);
      }
    };
    return {
      mouseHandler,
      bgColor,
      fontColor,
      isShow,
      delItem,
      isComp
    };
  }
});
</script>
<style scoped>
/* item */
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li buttom {
  float: right;

  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

.btn.btn-danger {
  float: right;
}
</style>