<template>
  <div>
    <h2 v-if="!isEditing" @click="edit">{{ note.title }}</h2>
    <input v-else v-model="editedNote.title" type="text" />
    <ul>
      <todo-item v-for="todo in note.todos" :key="todo.id" :todo="todo" @edit="editTodo" />
    </ul>
    <button v-if="!isEditing" @click="edit">Редактировать</button>
    <button v-else @click="save">Сохранить</button>
    <button v-if="!isEditing" @click="deleteNote">Удалить</button>
    <button v-else @click="cancel">Отмена</button>
  </div>
</template>

<script>
import { defineComponent, ref, reactive } from 'vue';
import TodoItem from './TodoItem.vue';

export default defineComponent({
  components: {
    TodoItem
  },
  props: {
    note: {
      type: Object,
      required: true
    }
  },
  setup(props, { emit }) {
    const isEditing = ref(false);
    const editedNote = reactive({ ...props.note });

    const edit = () => {
      isEditing.value = true;
    };

    const save = () => {
      emit('edit', props.note.id, editedNote);
      isEditing.value = false;
    };

    const deleteNote = () => {
      emit('delete', props.note.id);
    };

    const cancel = () => {
      isEditing.value = false;
    };

    const editTodo = (todoId, newTodo) => {
      const index = editedNote.todos.findIndex(todo => todo.id === todoId);
      editedNote.todos.splice(index, 1, newTodo);
    };

    return {
      isEditing,
      editedNote,
      edit,
      save,
      deleteNote,
      cancel,
      editTodo
    };
  }
});
</script>
