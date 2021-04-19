<template>
  <v-app>
    <v-layout row wrap>
    
      <v-flex xs4>
        <v-card>
          <v-card-text class="px-0">
            <ul class="list-group">
              <li
                v-on:click.prevent="Clicked(item)"
                v-for="item in todoItems"
                :key="item.id"
                class="list-group-item list-group-item-success"
              >
                <TodoItem :username="item" />
              </li>
            </ul>
          </v-card-text>
        </v-card>
      </v-flex>

      <v-flex xs4>
        <v-card>
          <v-card-text class="px-0">
            <v-form ref="form1" v-on:submit.prevent="AddToDo">
              <div class="form-group">
                <label for="exampleTitle">Title</label>
                <v-text-field
                  v-model="itemTitle"
                  :rules="rules"
                  required
                ></v-text-field>
              </div>
              <button type="submit" class="btn btn-primary" id="todoID">
                Add
              </button>
            </v-form>
          </v-card-text>
        </v-card>
      </v-flex>

      <v-flex xs4>
        <v-card>
          <v-card-text class="px-0">
            <ul class="list-group">
              <li
                v-on:click.prevent="Clicked(ditem)"
                v-for="ditem in Ditems"
                :key="ditem.id"
                class="list-group-item list-group-item-danger"
              >
                <TodoItem :username="ditem" />
              </li>
            </ul>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>

    <v-container>
      <v-img
        alt="Vue logo"
        max-height="200"
        max-width="200"
        src="../assets/logo.png"
      ></v-img>

      <hr />
      
     
      <div>
        <input type="text" name="" id="del" placeholder="Order Key" />
        <v-btn color="error" v-on:click.prevent="DeleteToDo"> Delete</v-btn>
      </div>
      <hr />
     
     
     
  
    </v-container>

    <v-footer app>
      <!-- -->
    </v-footer>
  </v-app>
</template>

<script>
import TodoItem from "./TodoItem";
import items from "../data/items";


export default {
  name: "TodoListComponent",
  components: {
    TodoItem,
  
  },
  data: function () {
    return {
      itemTitle: "",
      rules: [
        (value) => !!value || "Required.",
        (value) => (value && value.length >= 3) || "Min 3 characters",
      ],
      todoItems: items,
      Ditems: [],
      updateItem: 0,
    };
  },
  methods: {
    DeleteToDo: function () {
      const del = document.getElementById("del").value;
      const index = this.todoItems.findIndex((item) => item.id == del);

      if (index > 0) {
        this.Ditems.push(this.todoItems[index]);

        this.todoItems.splice(index, 1);
        console.log(this.todoItems.length);
        console.log(this.Ditems.length);
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
        // document.getElementById('exampleTitle').value = null;
        // document.getElementById('todoID').innerText = "Add";
        this.updateItem = 0;
      } else {
        let itemsLenght = this.todoItems.length + this.Ditems.length;

        console.log(itemsLenght);

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
      }
    },
    Clicked: function (item) {
      document.getElementById("exampleTitle").value = item.title;
      document.getElementById("todoID").innerText = "Update";
      this.updateItem = item.id;
    },
  },
};
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>