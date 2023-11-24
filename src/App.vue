<template>
  <div class="container">
    <div class="header">
      <h1 class="h1">TodoList App</h1>
    </div>
    <button class="btn mrgt" @click="showModal">Создать заметку</button>
    <the-modal :visible="modalVisible" @visible="closeModal">
      <note-form @create="createNote" />
    </the-modal>
    <div v-if="notes.length">
      <my-note
        v-for="note in notes"
        :key="note.id"
        :note="note"
        @delete="deleteNote"
        @deleteTodo="deleteTodo"
        @edit="editNote"
        @editTodo="editTodo"
      />
    </div>
    <div v-else>Заметок нет</div>
  </div>
</template>

<script>
import { defineComponent, ref } from "vue"
import TheModal from "./ui/TheModal.vue"
import NoteForm from "./components/NoteForm.vue"

import MyNote from "@/components/MyNote.vue"

export default defineComponent({
  components: {
    MyNote,
    TheModal,
    NoteForm,
  },
  setup() {
    const notes = ref([
      {
        id: "46e79ea8-4425-4f60-a13f-471c429e5731",
        title: "Заметка 1",
        todos: [
          { id: "1t", text: "Пойти на прогулку", done: true },
          { id: "1ts", text: "Гулять 2 часа", done: false },
        ],
      },
    ])
    const modalVisible = ref(false)

    function showModal() {
      modalVisible.value = true
    }
    function closeModal() {
      modalVisible.value = false
    }
    function createNote(note) {
      if (note.title.length > 0) {
        notes.value.push(note)
      }
      modalVisible.value = false
    }
    function deleteNote(id) {
      const index = notes.value.findIndex((note) => note.id === id)
      if (index !== -1) {
        notes.value.splice(index, 1)
      }
    }
    function deleteTodo(id, idP) {
      console.log("1", id, idP)

      const index = notes.value.findIndex((note) => note.id === idP)
      console.log("2", index)
      const indexT = notes.value[index].todos.findIndex(
        (note) => note.id === id
      )
      console.log("3", indexT)

      if (indexT !== -1) {
        notes.value[index].todos.splice(indexT, 1)
      }
    }
    function editNote(newNote) {
      const index = notes.value.findIndex((n) => n.id === newNote.id)
      notes.value.splice(index, 1, newNote)
    }

    return {
      notes,
      modalVisible,
      showModal,
      closeModal,
      createNote,
      deleteNote,
      deleteTodo,
      editNote,
    }
  },
})
</script>
<style scoped>
.header {
  display: flex;
  padding: 10px;
  border-bottom: var(--border-table);
}
.h1 {
  font: var(--font-l);
  font-weight: 700;
  margin: 0 auto;
}
.mrgt {
  margin-top: 15px;
}
</style>
