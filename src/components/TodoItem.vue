<template>
  <li>
    <input type="checkbox" v-model="todo.done">
    <span>{{ todo.text }}</span>
  </li>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  props: {
    todo: {
      type: Object,
      required: true
    }
  }
});
</script>