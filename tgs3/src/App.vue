<template>
  <div id="app">
    <h1>To Do List</h1>
    <h2>Ellsya Putri Ananda</h2>
    <form @submit.prevent="addTask">
      <input v-model="newTask" placeholder="Add a new task" />
      <button type="submit">Add</button>
    </form>
    <ul>
      <li v-for="task in tasks" :key="task.id">
        <div v-if="task.id === editId">
          <input v-model="editTask" />
          <button @click="updateTask(task.id)">Save</button>
          <button @click="cancelEdit">Cancel</button>
        </div>
        <div v-else>
          {{ task.name }}
          <button @click="setEditTask(task.id, task.name)">Edit</button>
          <button @click="deleteTask(task.id)">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: JSON.parse(localStorage.getItem('tasks') || '[]'),
      editId: null,
      editTask: ''
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() === '') return;

      const newTaskObj = {
        id: Date.now(),
        name: this.newTask.trim()
      };

      this.tasks.push(newTaskObj);
      this.newTask = '';
      this.saveTasks();
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
      this.saveTasks();
    },
    setEditTask(id, name) {
      this.editId = id;
      this.editTask = name;
    },
    updateTask(id) {
      const task = this.tasks.find(task => task.id === id);
      if (task) {
        task.name = this.editTask;
        this.saveTasks();
        this.cancelEdit();
      }
    },
    cancelEdit() {
      this.editId = null;
      this.editTask = '';
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    }
  }
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #a0ddeb;
}

#app {
  max-width: 600px;
  margin: 50px auto;
  padding: 20px;
  background: #ebaede;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #333;
}

form {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

input[type="text"] {
  padding: 10px;
  border: 1px solid #95e7b3;
  border-radius: 4px;
  margin-right: 10px;
  flex: 1;
}

button {
  padding: 10px;
  border: none;
  border-radius: 4px;
  background: #9ae3f0;
  color: rgb(255, 255, 255);
  cursor: pointer;
}

button:hover {
  background: #0056b3;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background: #f9f9f9;
  margin-bottom: 10px;
  border-radius: 4px;
}

li div {
  display: flex;
  align-items: center;
}

button + button {
  margin-left: 10px;
}
</style>
