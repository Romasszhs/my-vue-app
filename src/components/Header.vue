<template>
  <div class="todo-header">
    <input type="text"  placeholder="请输入你的任务名称，按回车键确认" v-model="title" @keyup.enter="add" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
export default defineComponent({
  name: 'Header',
  props: {
    addTodo: {
      type: Function,
      required: true,// 必须传参
    }
  },
  setup(props) {
    // 定义一个ref类型的数据
    const title = ref('')
    const num = 1

    // 回车的事件的回调函数，用来添加数据
    const add = () => {
      // 获取文本框中输入的数据，判断不能为空
      console.log(num)
      console.log(title)
      const text = title.value

      console.log(text.trim())
      console.log(!text.trim())

      // 对字符串取反，只有空格字符串会返回true
      if (!text.trim()) return
      // 此时有数据，创建一个todo对象
      const todo = {
        id: Date.now(),
        title: text.trim(),
        isCompleted: false
      }
      // 调用addTodos方法
      props.addTodo(todo)
      // 清空文本框
      title.value = ''
    }


    return {
      title,
      add,
    }
  },
})
</script>

<style scoped>
/*header*/
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
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
}
</style>