<template>
  <form class="todo-inner" @submit.prevent="onSubmit" autocomplete="off">
    <input
      class="input"
      type="text"
      v-model="text"
      placeholder="Введите todo"
      @keyup.enter="onSubmit"
      @blur="onSubmit"
      autocomplete="off"
    />
    <button class="btn-small" type="submit">+</button>
  </form>
</template>

<script>
import { defineComponent, ref } from "vue"
import { v4 as uuidv4 } from "uuid"

export default defineComponent({
  setup(_, { emit} ) {
    const id = ref("")
    const text = ref("")
    const done = ref("false")

    function onSubmit() {
      emit("createItem", {
        id: uuidv4(),
        text: text.value,
        done: done.value,
      })
      id.value = ""
      text.value = ""
      done.value = false
    }

    return {
      onSubmit,
      id,
      text,
      done,
    }
  },
})
</script>

<style scoped>
.todo-inner {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  margin-top: 10px;
  width: 100%;
}
</style>
