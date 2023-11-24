<template>
  <li class="inner">
    <input type="checkbox" v-model="todo.done" :disabled="!isEditing" />
    <span v-if="!isEditing">{{ todo.text }}</span>
    <input
      v-else
      v-model="newText"
      @blur="confirmEditing"
      @keyup.enter="confirmEditing"
    />
    <button class="btn-small mrg" @click="startEditing" v-if="!notButtons">
      <AppIcon name="editIcon" />
    </button>
    <button class="btn-small" @click="deleteTodo" v-if="!notButtons">
      <AppIcon name="trashIcon" />
    </button>
  </li>
</template>

<script>
import { defineComponent, ref } from "vue"

import AppIcon from "../ui/AppIcon.vue"

export default defineComponent({
  components: {
    AppIcon,
  },
  props: {
    todo: {
      type: Object,
      required: true,
    },
    notButtons: {
      type: Boolean,
      default: false,
    },
  },
  setup(props, { emit }) {
    const isEditing = ref(false)
    const newText = ref(props.todo.text)

    function deleteTodo() {
      const isDelete = confirm("Удалить?")
      if (isDelete) {
        emit("deleteTodo", props.todo.id)
      }
    }

    function confirmEditing() {
      props.todo.text = newText.value

      emit("editTodo", props.todo)
      isEditing.value = false
    }
    const startEditing = () => {
      isEditing.value = true
    }

    return {
      deleteTodo,
      startEditing,
      isEditing,
      newText,
      confirmEditing,
    }
  },
})
</script>

<style scoped>
.inner {
  display: flex;
  gap: 5px;
  align-items: center;
}
.mrg {
  margin-left: auto;
}
</style>
