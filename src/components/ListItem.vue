<template>
  <li p="y-2 l-4 r-2" flex="~" :style="todo.done ? 'opacity: 0.25' : 'opacity: 1.0'">
    <p flex="1">{{ todo.text }}</p>
    <label m="l-2" flex="~" items="center">
      <button @click="$emit('deleteItem')" m="r-2">🗑️</button>
      <input
        type="checkbox"
        form="checkbox"
        h="5"
        w="5"
        text="yellow-600"
        bg="gray-500"
        border="rounded"
        v-model="todo.done"
        @change="$emit('movePosition')"
      />
    </label>
  </li>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  props: ["todo"],
  emits: ["movePosition", "deleteItem"],
});
</script>