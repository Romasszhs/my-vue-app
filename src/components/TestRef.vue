<template>
    <h2>App</h2>
    全名<input type="text">
    小名<input type="text" ref="inputRef" v-model="littleName">
</template>

<script lang="ts">
import { onMounted, ref, defineComponent } from 'vue'
import { watch } from 'vue'
import { computed } from 'vue'
/* 
ref获取元素: 利用ref函数获取组件中的标签元素
功能需求: 让输入框自动获取焦点
*/
export default defineComponent({
    name: 'TestRef',
    setup() {
        const littleName = ref<string>('小昵称')
        const inputRef = ref<HTMLElement | null>(null)

        onMounted(() => {
            inputRef.value && inputRef.value.focus()
        })
        watch(littleName, (newValue, oldValue) => {
            console.log("数据变化了", `新值是：${newValue}`, `旧值是${oldValue}`)
        })

        return {
            inputRef,
            littleName,
        }
    }
})
</script>