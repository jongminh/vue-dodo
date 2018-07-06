<template>
  <v-container fluid>
    <todo-new-item @new-todo-item="addNewItem"/>  
    <v-toolbar color="cyan" dark>
      <v-toolbar-title>Todo items</v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
    <v-list three-line>
      <template v-for="item in orderedTodoList">
        <v-list-tile :key="item.id">
          <v-list-tile-action>
            <v-checkbox v-model="item.done"></v-checkbox>
          </v-list-tile-action>
          <v-list-tile-content @click="item.done = !item.done">
            <div v-bind:class="{ isSelected: item.done }">
              <v-list-tile-title>{{item.title}}</v-list-tile-title>
            <v-list-tile-sub-title>{{item.content}}</v-list-tile-sub-title>
            </div>
          </v-list-tile-content>
          <v-list-tile-action>
            <v-btn icon ripple v-on:click="deleteItem(item.id)">
              <v-icon color="grey lighten-1">delete</v-icon>
            </v-btn>
          </v-list-tile-action>
        </v-list-tile>
      </template>
    </v-list>
    <div>
      <v-alert
        :value="todoList.length === 0"
        color="info"
        icon="info"
        outline
      >
        There is no Todo Item.
    </v-alert>
    </div>
  </v-container>
</template>

<script>
import TodoNewItem from "./TodoNewItem";
import _ from "lodash";

export default {
  name: "TodoList",
  components: {
    TodoNewItem
  },
  data() {
    return {
      todoList: [],
      lastId: 0
    };
  },
  mounted() {
    if(localStorage.getItem('todoList')) {
      try {
        this.todoList = JSON.parse(localStorage.getItem('todoList'));
      } catch(e) {
        localStorage.removeItem('todoList');
        localStorage.lastId = 0;
      }
    }
    if(localStorage.lastId) {
      this.lastId = localStorage.lastId;
    }
  },
  methods: {
    addNewItem(newItem) {
      this.lastId = this.lastId + 1;
      const newTodoItem = { ...newItem, done: false, id: this.lastId };
      this.todoList.push(newTodoItem);
      this.saveTodoList();
    },
    deleteItem(itemId) {
      const selecetedItem = _.find(this.todoList,['id', itemId])
      const modifiedTodoList =  _.pull(this.todoList, selecetedItem )
      this.todoList = [...modifiedTodoList]
      this.saveTodoList();
    },
    saveTodoList() {
      let parsed = JSON.stringify(this.todoList);
      localStorage.setItem('todoList', parsed);
      localStorage.lastId = this.lastId;
    }
  },
  computed: {
    orderedTodoList() {
      return _.orderBy(this.todoList, ["id"], ["desc"]);
    }
  }
};
</script>

<style scoped>
.isSelected {
  text-decoration: line-through;
}
</style>