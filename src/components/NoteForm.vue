<template>
  <form class="form-wrapper" @submit.prevent="onSubmit" autocomplete="off">
    <input
      class="input"
      type="text"
      id="note"
      v-model="note.title"
      placeholder="Введите название заметки"
      autocomplete="off"
    />

    <ul class="ul-wrapper">
      <todo-item
        v-for="todo in note.todos"
        :key="todo.text"
        :todo="todo"
        :notButtons="true"
      />
    </ul>
    <todo-form @createItem="addChild"></todo-form>
    <button class="btn mrgt" :disabled="!isValid" type="submit">
      Добавить
    </button>
  </form>
</template>

<script>
import { defineComponent, computed, reactive } from "vue"
import { v4 as uuidv4 } from "uuid"

import TodoItem from "./TodoItem.vue"
import TodoForm from "./TodoForm.vue"

export default defineComponent({
  components: {
    TodoItem,
    TodoForm,
  },
  setup(_, { emit} ) {
    const note = reactive({
      id: "",
      title: "",
      todos: [],
    })

    function onSubmit() {
      note.id = uuidv4()
      emit("create", note)
    }

    const isValid = computed(() => {
      return note.title.length > 0
    })
    function addChild(todo) {
      if (todo.text !== "") {
        note.todos.push(todo)
      }
    }

    return {
      note,
      onSubmit,
      isValid,
      addChild,
    }
  },
})
</script>

<style scoped>
.form-wrapper {
  display: flex;
  flex-direction: column;
}
.mrgt {
  margin-top: 15px;
}
</style>
