<template>
  <div>
    <div>
      <button @click="increment">增加</button>
    </div>
    <p>{{ state.count }}, double: {{ state.double }}, watchCount: {{ state.watchCount }}</p>
    <p>refValue: {{ refValue }}</p>
  </div>
</template>

<script>
// 不能 import { reactive, computed } from '@vue/composition-api'
import { ref, reactive, computed, watch, onMounted } from '@vue/composition-api';


export default {
  name: 'BasicUse',
  setup() {
    const state = reactive({
      count: 0,
      // computed返回一个ref, 在reactive中会自动unRef
      double: computed(() => state.count * 2),
      watchCount: 0
    })

    const refValue = ref(0)

    const increment = () => {
      state.count++
      // ref值取值时需要用.value,在模板中不需要
      refValue.value++
    }

    // watch函数自动观察依赖的值触发副作用
    watch(() => {
      state.watchCount = state.count
    })

    // 只能在setup函数中调用
    onMounted(() => state.count++)

    return {
      state,
      increment,
      refValue
    }
  }
}
</script>

<style>

</style>
