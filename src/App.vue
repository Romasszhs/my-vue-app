<template>
    <div class="todo-container">
        <div class="todo-wrap">
            购车调查
            <Header :addTodo="addTodo" />
            <List :todos="todos" :deleteTodo="deleteTodo" :updateTodo="updateTodo" />
            <Footer :todos="todos" :checkAll="checkAll" :clearAllCompletedTodos="clearAllCompletedTodos" />
            <Test />
            <TestRef />
        </div>
    </div>
</template>
<script lang="ts">
import { defineComponent, onMounted, reactive, toRefs, watch } from 'vue'
// 引入直接的子级组件
import Header from './components/Header.vue'
import List from './components/List.vue'
import Footer from './components/Footer.vue'
import Test from './components/Test.vue'
import TestRef from './components/TestRef.vue'


// 引入接口
import { Todo, Todo2 } from './types/todo.ts'
// 引入函数
import { saveTodos, readTodos } from './utils/localStorageUtils.ts'

export default defineComponent({
    name: 'App',
    // 注册组件
    components: {
        Header,
        List,
        Footer,
        Test,
        TestRef
    },
    // 数据应该用数组来存储,数组中的每个数据都是一个对象,对象中应该有三个属性(id,title,isCompleted)
    // 把数组暂且定义在App.vue父级组件

    setup() {
        // 定义一个数组数据
        const state = reactive<{ todos: Todo[] }>({
            todos: [
                // { id: 1, title: 'benz', isCompleted: false },
                // { id: 2, title: 'Honda', isCompleted: true },
                // { id: 3, title: 'Li', isCompleted: false }
            ]
        })

        // 增加添加数据的方法
        const addTodo = (todo: Todo) => {
            state.todos.unshift(todo)
        }

        // 删除数据的方法
        const deleteTodo = (index: number) => {
            state.todos.splice(index, 1)
        }
        // 修改todo的状态isCompleted属性的状态
        const updateTodo = (todo: Todo, isCompleted: boolean) => {
            todo.isCompleted = isCompleted
            console.log(todo)
        }
        // 全选或全不选的方法
        const checkAll = (isCompleted: boolean) => {
            // 遍历数组
            state.todos.forEach(todo => {
                todo.isCompleted = isCompleted
            });
        }
        // 清理所有选中的数据
        const clearAllCompletedTodos = () => {
            state.todos = state.todos.filter(todo => !todo.isCompleted)
        }

        // 监视操作：如果todos数组的数据变化了，直接存储到浏览器的缓存中
        // watch(()=>state.todos,(value)=>{
        //   // 保存到浏览器的缓存中
        //   localStorage.setItem('todos_key',JSON.stringify(value))
        // },{deep:true})

        watch(() => state.todos, (value) => {
            // 保存到浏览器的缓存中
            saveTodos(value)
        }, { deep: true })

        watch(() => { return state.todos }, saveTodos, { deep: true })

        // 界面加载完毕后过一段时间进行数据读取
        onMounted(() => {
            setTimeout(() => {
                state.todos = readTodos()
            }, 1000)
        })

        console.log(state.todos.length)


        return {
            ...toRefs(state),
            addTodo,
            deleteTodo,
            updateTodo,
            checkAll,
            clearAllCompletedTodos,
        }
    },
})
</script>
<style scoped>
/*app*/
.todo-container {
    width: 600px;
    margin: 0 auto;
}

.todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
}
</style>