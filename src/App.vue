<template>
  <div id="app">
    <div class="container">
      <img alt="Vue logo" src="./assets/logo.png">
    </div>
      <div class="row">
        <div class="col">
          <ul class="list-group">
            <li v-on:click.prevent="Clicked(item)" v-for="item in todoItems" :key="item.id" class="list-group-item list-group-item-success">
              <TodoItem  :username="item"/>
            </li>
          </ul>
        </div>
        <div class="col">
          <form v-on:submit.prevent="AddToDo">
            <div class="form-group">
              <label for="exampleTitle">Title</label>
              <input type="text" class="form-control" id="exampleTitle" placeholder="Enter Title">
            </div>
            <button type="submit" class="btn btn-primary" id="todoID">Add</button>
          </form>
        </div>
        <div class="col">
          <ul class="list-group">
            <li v-on:click.prevent="Clicked(ditem)" v-for="ditem in Ditems" :key="ditem.id" class="list-group-item list-group-item-danger">
              <TodoItem :username="ditem"/>
            </li>
          </ul>
        </div>
      </div>
      <hr />
      <div>
        <input type="text" name="" id="del" placeholder="Order Key">
        <button v-on:click.prevent="DeleteToDo" > Delete</button>
      </div>
      <hr />
  </div>
</template>

<script>
import TodoItem from './components/UserComponent';
import items from './data/items';

export default {
  name: 'App',
  components: {
    TodoItem
  },
  data: function () {
    return {
      
      todoItems: items,
      Ditems: [],
      updateItem: 0
    }
  },
  methods: {
    DeleteToDo: function(){
      const del = document.getElementById('del').value;
      const index = this.todoItems.findIndex(item => item.id == del);

      if(index > 0){
        this.Ditems.push(this.todoItems[index]);  
      
        this.todoItems.splice(index, 1);
        console.log(this.todoItems.length);
        console.log(this.Ditems.length);
      } else {
        window.alert("Girilen Değerde Order Bulunamadı")
      }
    },
    AddToDo: function(){
      const title = document.getElementById('exampleTitle').value;
      console.log(title);

      if(this.updateItem > 0){
        const index = this.todoItems.findIndex(item => item.id == this.updateItem);
        this.todoItems[index].title = title;

        document.getElementById('exampleTitle').value = null;
        document.getElementById('todoID').innerText = "Add";
        this.updateItem = 0;
      }else{
        let itemsLenght = this.todoItems.length + this.Ditems.length;

        console.log(itemsLenght);

        const date = new Date();
        const djson = JSON.stringify(date);

        const newitem = [{
          id: itemsLenght + 1,
          title: title,
          order: itemsLenght + 1,
          completed: false,
          createdOn: djson
        }];

        this.todoItems.push(newitem[0]);
        document.getElementById('exampleTitle').value = null;
      }
    },
    Clicked: function(item) {
      document.getElementById('exampleTitle').value = item.title;
      document.getElementById('todoID').innerText = "Update";
      this.updateItem = item.id;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  
}

</style>
