<template>
  <div>
    <div class="first-wrapper">
      <AppIcon name="folderIcon" />
      <h2 class="h2" v-if="!isEditing">{{ note.title }}</h2>
      <input
        v-else
        v-model="newTitle"
        @blur="confirmEditing"
        @keyup.enter="confirmEditing"
      />

      <button class="btn-small mrg" @click="startEditing">
        <AppIcon name="editIcon" />
      </button>
      <button class="btn-small" @click="deleteNote">
        <AppIcon name="trashIcon" />
      </button>
    </div>
    <ul class="ul-wrapper">
      <todo-item
        v-for="todo in note.todos"
        :key="todo.id"
        :todo="todo"
        @deleteTodo="deleteTodo"
      />
    </ul>
  </div>
</template>

<script>
import { defineComponent, ref } from "vue"
import TodoItem from "./TodoItem.vue"
import AppIcon from "../ui/AppIcon.vue"

export default defineComponent({
  components: {
    TodoItem,
    AppIcon,
  },
  props: {
    note: {
      type: Object,
      required: true,
    },
  },
  setup(props, { emit }) {
    const isEditing = ref(false)
    const newTitle = ref(props.note.title)

    function deleteNote() {
      const isDelete = confirm("Удалить?")
      if (isDelete) {
        emit("delete", props.note.id)
      }
    }
    function deleteTodo(payload) {
      emit("deleteTodo", payload, props.note.id)
    }

    function confirmEditing() {
      props.note.title = newTitle.value
      emit("edit", props.note)
      isEditing.value = false
    }

    const startEditing = () => {
      isEditing.value = true
    }

    return {
      deleteNote,
      deleteTodo,
      startEditing,
      isEditing,
      newTitle,
      confirmEditing,
    }
  },
})
</script>
<style scoped>
.first-wrapper {
  margin-top: 20px;
  display: flex;
  gap: 5px;
  align-items: center;
}

.mrg {
  margin-left: auto;
}
</style>
