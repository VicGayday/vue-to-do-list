<template>
  <div>
    <my-note v-for="(note, index) in notes.value" :key="note.id" :note="note" :isEditing="editingNoteId === note.id" @delete="deleteNote(index)" @edit="editNote(index, $event)" @save="saveNote" @cancel="cancelEditing" />
    <!-- здесь будут кнопки действий -->
  </div>
</template>

<script>
import { defineComponent, ref } from 'vue';
import MyNote from '@/components/MyNote.vue';

export default defineComponent({
  components: {
    MyNote
  },
  setup() {
    const notes = ref([{id: 1, title: "Заметка 1", todos: [{id: "1t", text: "Пойти на прогулку", done: false}]}]);
    const editingNoteId = ref(null);
    const deleteNote = (index) => {
      notes.value.splice(index, 1);
    };
    const editNote = (index, newNote) => {
      editingNoteId.value = notes.value[index].id;
    };
    const saveNote = (newNote) => {
      const index = notes.value.findIndex(note => note.id === editingNoteId.value);
      notes.value.splice(index, 1, newNote);
      editingNoteId.value = null;
    };
    const cancelEditing = () => {
      editingNoteId.value = null;
    };
    // Здесь будут другие методы для работы с заметками

    return {
      notes,
      editingNoteId,
      deleteNote,
      editNote,
      saveNote,
      cancelEditing
    };
  }
});
</script>
