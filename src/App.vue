<template>
<v-app>
    <AppBarComponent />

  <todo-list-component />
    

</v-app>
</template>


<script>
import TodoListComponent from "./components/TodoListComponent.vue";
import AppBarComponent from "./components/AppBarComponent.vue";

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
