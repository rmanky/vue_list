<template>
  <form @submit.prevent="addItem()" m="t-4" class="relative">
    <input
      bg="dark-600"
      color="light-900"
      w="full"
      p="2"
      border="rounded-lg none"
      type="text"
      placeholder="Achieve world domination..."
      v-model="textToAdd"
    />
    <button
      :disabled="textToAdd.length == 0"
      opacity="disabled:50"
      position="absolute"
      inset="y-0"
      right="0"
      p="x-4"
      bg="yellow-600"
      rounded="r-lg"
    >
      Add
    </button>
  </form>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  emits: ["addItem"],
  data() {
    return {
      textToAdd: "",
    };
  },
  methods: {
    addItem() {
      this.$emit("addItem", this.textToAdd);
      this.textToAdd = "";
    },
  },
});
</script>
