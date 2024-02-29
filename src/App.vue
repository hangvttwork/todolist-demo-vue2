<template>
  <div>
    <h1>Hello, I'm Hang Vu</h1>

    <h3>Todo list</h3>
    <button @click="addNewTodo()">Create a new todo</button>

    <div>
      <div v-for="(list, index) in newTodoList" :key="index">
        <label>Time: </label>
        <input type="text" v-model="list.time" />
        <button @click="addNewTask(index)">Add task</button>

        <div>
          <div v-for="(tasks, index) in list.tasks" :key="index">
            <input type="text" v-model="list.tasks[index]" />
          </div>
        </div>

        <button @click="saveTodo(index)">Save Todo</button>
      </div>
      <h3>{{ saveMessage }}</h3>
    </div>

    <button @click="fetchAllTodoList()">Fetch all saved Todolist</button>
    <h3>{{ fetchMessage }}</h3>
  </div>
</template>

<script>
import { createNewTodoList, fetchAllTodo } from "@/firebaseConfig";

export default {
  name: 'App',
  data() {
    return {
      newTodoList: [],
      saveMessage: '',
      fetchMessage: '',
    }
  },
  methods: {
    addNewTodo() {
      this.$set(this.newTodoList, this.newTodoList.length, {
        time: "",
        tasks: [],
      });
    },
    addNewTask(index) {
      this.$set(this.newTodoList, index, {
        time: "",
        tasks: [...this.newTodoList[index].tasks, ""],
      });
    },
    async saveTodo(index) {
      await createNewTodoList(this.newTodoList[index]);
      this.newTodoList.splice(index, 1);

      this.saveMessage = "Save successfully!";
    },
    async fetchAllTodoList () {
      await fetchAllTodo();
      this.fetchMessage = "Due to time limited, please see result in console log. Thanks";
    }
  }
}
</script>
