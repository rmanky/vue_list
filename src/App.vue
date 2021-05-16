<template>
  <h1 m="t-8" text="3xl">Good {{ getGreeting }}.</h1>
  <TodoList />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import TodoList from "./components/TodoList.vue";
import { SplashScreen } from "@capacitor/splash-screen";

export default defineComponent({
  name: "App",
  components: {
    TodoList,
  },
  computed: {
    getGreeting() {
      var today = new Date();
      var curHr = today.getHours();

      if (curHr < 12) {
        return "morning";
      } else if (curHr < 18) {
        return "afternoon";
      } else {
        return "evening";
      }
    },
  },
  mounted() {
    SplashScreen.hide();
  },
});
</script>

<style>
#app {
  @apply min-h-screen;
  @apply bg-dark-900;
  @apply text-light-900;
  @apply p-4;
  @apply text-lg;
}
</style>
