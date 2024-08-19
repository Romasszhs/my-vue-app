<template>
    <ul class="todo-main">
        <Item v-for="(todo, index) in todos" :key="todo.id" :todo="todo" :deleteTodo="deleteTodo"
            :updateTodo="updateTodo" :index="index" />
    </ul>
</template>

<script lang="ts">
import { defineComponent } from "vue";
// 引入子级组件
import Item from "./Item.vue";
// 引入axios
import axios from 'axios';
// 引入接口
import { Todo } from '../types/todo.ts'
export default defineComponent({
    name: 'List',
    components: {
        Item
    },
    // props:['todos','deleteTodo','updateTodo']
    props: {
        todos: {
            //type:Object as () => Todo, // 函数返回的是Todo类型
            type: Array as () => Todo[],
            required: true,
            default: () => ({})
        },
        deleteTodo: {
            type: Function,
            required: true
        },
        updateTodo: {
            type: Function,
            required: true
        },
    },
    setup(props) {
        // 获取远程url地址的data数据
        const getTodoList = () => {
            // 调用接口
            axios.get('https://www.runoob.com/jsref/jsref-operators.html').then((res) => {
                console.log(res.data)
            })
            // 使用axios获取post请求的data数据
            axios.post('http://localhost:3000/todoList', { name: '张三' }).then((res) => {
                //console.log(res.data)

            })
        }
        return{
            getTodoList
        }
    }
})

</script>

<style scoped>
/*main*/
.todo-main {
    margin-left: 0px;
    border: 1px solid #ddd;
    border-radius: 2px;
    padding: 0px;
}

.todo-empty {
    height: 40px;
    line-height: 40px;
    border: 1px solid #ddd;
    border-radius: 2px;
    padding-left: 5px;
    margin-top: 10px;
}
</style>