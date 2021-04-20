<template>
  <v-simple-table>
    <template>
      <thead>
        <tr>
          <th class="text-left">
            Order
          </th>
          <th class="text-left">
            Title
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-on:click="onItemClick(item)"
          v-for="item in tableItem"
          :key="item.id"
        >
          <td>{{ item.order }}</td>
          <td>{{ item.title }}</td>
        </tr>
      </tbody>
    </template>
  </v-simple-table>
</template>

<script>
  export default {
    props: {
      tableItem: Array,
    },
    data () {
      return {
        value: 'I am the child.', 
      }
    },
     methods:{
      onItemClick: function (item) {
        this.$emit('todoItemSelected', item)
      },
    } 
  }
</script>

<style>

</style>