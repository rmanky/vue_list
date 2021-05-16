<template>
  <p m="t-4" text="xl">
    You have
    <span text="yellow-600">{{ getTodosCount }}</span>
    {{ getTodosCount == 1 ? "thing" : "things" }} to do!
  </p>

  <AddBar @addItem="addItem" />

  <transition-group
    name="todo-list"
    tag="ul"
    m="t-4"
    p="0"
    bg="dark-600"
    border="rounded-lg"
    divide="y-2 dark-300"
  >
    <ListItem
      class="todo-list-item"
      v-for="(todo, idx) in todos"
      v-bind:todo="todo"
      v-bind:key="todo.id"
      @movePosition="movePosition(idx)"
      @deleteItem="deleteItem(idx)"
    />
  </transition-group>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import ListItem from "./ListItem.vue";
import AddBar from "./AddBar.vue";
import { v4 as uuidv4 } from "uuid";
import { Storage } from "@capacitor/storage";

export default defineComponent({
  name: "HelloWorld",
  components: {
    ListItem,
    AddBar,
  },
  data() {
    return {
      todos: [] as { text: string; done: boolean; id: string }[],
    };
  },
  async mounted() {
    const { value: firstTime } = await Storage.get({ key: "first" });
    if (firstTime != "true") {
      await Storage.set({
        key: "first",
        value: "true",
      });
      this.todos = [
        { text: "Learn Vue", done: false, id: uuidv4() },
        { text: "Fix Bugs", done: false, id: uuidv4() },
        { text: "Profit?", done: false, id: uuidv4() },
      ];
    }
    const { value: todos } = await Storage.get({ key: "todo_list" });
    if (todos) {
      this.todos = JSON.parse(todos);
    }
  },
  computed: {
    getTodosCount(): number {
      const filtered = this.todos.filter((e) => !e.done);
      return filtered.length;
    },
  },
  watch: {
    todos: {
      handler: async (list) => {
        await Storage.set({
          key: "todo_list",
          value: JSON.stringify(list),
        });
      },
      deep: true,
    },
  },
  methods: {
    addItem(textToAdd: string) {
      this.todos.unshift({
        text: textToAdd,
        done: false,
        id: uuidv4(),
      });
    },
    movePosition(idx: number) {
      const selected = this.todos.splice(idx, 1).pop();
      if (selected != undefined) {
        if (selected.done) {
          this.todos.push(selected);
        } else {
          this.todos.unshift(selected);
        }
      }
    },
    deleteItem(idx: number) {
      this.todos.splice(idx, 1);
    },
  },
});
</script>

<style>
.todo-list-item {
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.todo-list-enter-from,
.todo-list-leave-to {
  opacity: 0 !important;
  transform: scale(0.9);
}
</style>
