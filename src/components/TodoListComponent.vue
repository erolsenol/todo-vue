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
            <v-btn depressed rounded medium v-on:click="AddToDo" :color="isUpdating ? 'primary' : ''"> {{ isUpdating ? "Update" : "Add" }} </v-btn>
          </v-col>
          <v-col cols="auto">
            <v-btn
            v-if="isUpdating"
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
import Table from "./Table";
import axios from "axios";

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
      todoItems: [],
    };
  },
  mounted: async function() {
      this.todoItems = (await axios.get("https://todowebapi.herokuapp.com/items")).data;
  },
  computed: {
    isUpdating: function() {
      return this.updateItem > 0
    }
  },
  watch: {

  },
  methods: {
    itemSelected(item) {
      this.itemTitle = item.title;
      this.updateItem = item.id;
    },
    DeleteToDo: async function (updateItem) {
      if (this.updateItem > 0) {
        const res = await axios.delete("https://todowebapi.herokuapp.com/items/" + this.updateItem);
        if(await res.status == 204){
          this.todoItems.splice((updateItem), 1);
          this.updateItem = 0;
          this.itemTitle = "";
        }
      } else {
        window.alert("Girilen Değerde Order Bulunamadı");
      }
    },
    AddToDo: async function () {
      if (this.updateItem > 0) {
        var postData = {
          'title': this.itemTitle
        };

        const res = await axios.put("https://todowebapi.herokuapp.com/items/" + this.updateItem, postData);
        if(res.status == 204){
          this.todoItems[(this.updateItem - 1)].title = postData.title;
          this.itemTitle = "";
          this.updateItem = 0;
        }
      } else if (this.itemTitle.length > 3) {
        const postData = {
          title: this.itemTitle
        };
        const res = await axios.post("https://todowebapi.herokuapp.com/items", postData);
        if(res.status == 201){
          this.todoItems.push(res.data);
          this.itemTitle = null;
        } else {
          window.alert("Hata Kodu " + res.status);
        }
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