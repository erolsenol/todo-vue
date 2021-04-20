<template>
  <v-main>
    <div>
      <v-container>
        <v-row>
          <v-col xs="12" sm="10" md="7" lg="6" xl="5" cols="auto">
            <Table
              :tableItem="todoItems"
              v-on:todoItemSelected="itemSelected"
              v-on:click.prevent="Clicked(item)"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="auto" xs="8" sm="6" md="5" lg="4" xl="3">
            <v-text-field
              label="Title"
              v-model="itemTitle"
              :itemTitle="rules"
              required
              hide-details="auto"
            ></v-text-field>
          </v-col>

          <v-col cols="auto">
            <v-btn depressed rounded medium v-on:click="AddToDo"> Add </v-btn>
          </v-col>
          <v-col cols="auto">
            <v-btn
              depressed
              rounded
              medium
              color="error"
              v-on:click="DeleteToDo(updateItem)"
              :disable="!!itemTitle"
            >
              Delete
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
    </div>
  </v-main>
</template>

<script>
import items from "../data/items";
import Table from "./Table";

export default {
  name: "TodoListComponent",
  components: {
    Table,
  },
  props: {},
  data: function () {
    return {
      itemTitle: "",
      updateItem: 0,
      rules: [
        (value) => !!value || "Required.",
        (value) => (value && value.length >= 3) || "Min 3 characters",
      ],
      todoItems: items,
      Ditems: [],
    };
  },
  methods: {
    itemSelected(item) {
      this.itemTitle = item.title;
      this.updateItem = item.id;
    },
    DeleteToDo: function (updateItem) {
      if (this.updateItem > 0) {
        this.todoItems.splice(updateItem, 1);
        this.updateItem = 0;
        this.itemTitle = "";
      } else {
        window.alert("Girilen Değerde Order Bulunamadı");
      }
    },
    AddToDo: function () {
      if (this.updateItem > 0) {
        const index = this.todoItems.findIndex(
          (item) => item.id == this.updateItem
        );
        this.todoItems[index].title = this.itemTitle;

        this.itemTitle = "";
        this.updateItem = 0;
      } else if (this.itemTitle.length > 3) {
        let itemsLenght = this.todoItems.length;

        const date = new Date();
        const djson = JSON.stringify(date);

        const newitem = [
          {
            id: itemsLenght + 1,
            title: this.itemTitle,
            order: itemsLenght + 1,
            completed: false,
            createdOn: djson,
          },
        ];

        this.todoItems.push(newitem[0]);
        this.itemTitle = null;
      } else {
        window.alert("En az 4 karakter olmalıdır title");
      }
    },
    Clicked: function (item) {
      this.updateItem = item.id;
    },
  },
};
</script>

<style scoped>

</style>