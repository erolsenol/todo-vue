<template>
  <v-main>
    <app-bar-component />

    <todo-list-component />
  </v-main>
</template>


<script>
import TodoListComponent from "./components/TodoListComponent.vue";
import AppBarComponent from "./components/AppBarComponent.vue";

// import items from './data/items';

export default {
  name: "App",
  components: {
    AppBarComponent,
    TodoListComponent,
  },
  data: function () {
    return {};
  },
  methods: {},
};
</script>

<style>
</style>
