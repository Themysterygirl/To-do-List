<template>
  <div class="todo-list">
    <h1 class="heading">My To-Do List</h1>
    <div class="input-group">
      <input type="text" v-model="newTask" @keyup.enter="addTask" placeholder="Add a new task...">
      <button @click="addTask" class="add-button">Add</button>
    </div>
    <h2 class="section-heading">Planned</h2>
    <ul>
      <li v-for="task in plannedTasks" :key="task.id" class="task-item">
        <div class="task-info">
          <span @click="editTask(task)" :class="{ 'completed': task.completed }" class="task-title">{{ task.title }}</span>
          <span class="created-date">(Created: {{ formatDate(task.created_at) }})</span>
        </div>
        <div class="button-group">
          <button @click="completeTask(task, 'completed')" class="task-button">Complete</button>
          <button @click="completeTask(task, 'inProgress')" class="task-button">Start</button>
          <button @click="deleteTask(task)" class="task-button">Delete</button>
        </div>
      </li>
    </ul>
    <h2 class="section-heading">In Progress</h2>
    <ul>
      <li v-for="task in inProgressTasks" :key="task.id" class="task-item">
        <div class="task-info">
          <span @click="editTask(task)" :class="{ 'completed': task.completed }" class="task-title">{{ task.title }}</span>
          <span class="created-date">(Created: {{ formatDate(task.created_at) }})</span>
        </div>
        <div class="button-group">
          <button @click="completeTask(task, 'completed')" class="task-button">Complete</button>
          <button @click="deleteTask(task)" class="task-button">Delete</button>
        </div>
      </li>
    </ul>
    <div class="task-info">
      <h2 class="section-heading">Completed Tasks</h2>
      <ul>
        <li v-for="task in completedTasks" :key="task.id" class="task-item">
          <div class="task-info">
            <span @click="editTask(task)" class="completed-task">{{ task.title }}</span>
            <span class="created-date">(Created: {{ formatDate(task.created_at) }})</span>
          </div>
          <div class="button-group">
            <button @click="deleteTask(task)" class="completed-task-button">Delete</button>
          </div>
        </li>
      </ul>
      <div class="centered-button-group">
        <button @click="clearCompletedTasks" class="clear-button">Clear Completed Tasks</button>
        <div class="spacer"></div>
        <button @click="clearAllTasks" class="clear-button">Clear All Tasks</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [],
      editedTask: null,
    };
  },
  computed: {
    plannedTasks() {
      return this.tasks.filter(task => task.status === 'planned');
    },
    inProgressTasks() {
      return this.tasks.filter(task => task.status === 'inProgress');
    },
    completedTasks() {
      return this.tasks.filter(task => task.completed);
    }
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== '') {
        const currentDate = new Date().toLocaleDateString();
        const newTask = { id: Date.now(), title: this.newTask, completed: false, created_at: currentDate, status: 'planned' };
        this.tasks.push(newTask);
        this.saveTasks(); // Save tasks to localStorage
        this.newTask = '';
      }
    },
    completeTask(task, targetStatus) {
      task.status = targetStatus;
      if (targetStatus === 'completed') {
        task.completed = true;
      }
      this.saveTasks(); // Save tasks to localStorage
    },
    deleteTask(task) {
      const index = this.tasks.findIndex(t => t.id === task.id);
      if (index !== -1) {
        this.tasks.splice(index, 1);
        this.saveTasks(); // Save tasks to localStorage
      }
    },
    clearCompletedTasks() {
      this.tasks = this.tasks.filter(task => !task.completed);
      this.saveTasks(); // Save tasks to localStorage
    },
    clearAllTasks() {
      localStorage.removeItem('tasks'); // Clear tasks from localStorage
      this.tasks = [];
    },
    editTask(task) {
      this.editedTask = task;
      this.newTask = task.title;
    },
    saveTasks() {
  try {
    localStorage.setItem('tasks', JSON.stringify(this.tasks));
  } catch (error) {
    console.error('Error saving tasks to localStorage:', error);
    // Handle the error, e.g., notify the user or log it
  }
},

    loadTasks() {
  try {
    const storedTasks = localStorage.getItem('tasks');
    if (storedTasks) {
      this.tasks = JSON.parse(storedTasks);
    }
  } catch (error) {
    console.error('Error loading tasks from localStorage:', error);
    // Handle the error, e.g., notify the user or log it
  }
},

    formatDate(dateString) {
      return new Date(dateString).toLocaleDateString();
    },
  },
  created() {
  this.loadTasks();
},

};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700&display=swap');

.todo-list {
  font-family: 'Playfair Display', serif;
  max-width: 600px;
  margin: auto;
  padding: 20px;
   /* Update font-family for the entire app */
}

.heading {
  font-size: 24px;
  margin-bottom: 20px;
  color: #333;
}

.section-heading {
  font-size: 20px;
  margin-top: 30px;
  margin-bottom: 15px;
  color: #666;
}

.task-title {
  cursor: pointer;
  color: #222;
}

.completed-task {
  text-decoration: line-through;
  color: #999;
  cursor: pointer;
}

.created-date {
  margin-left: 10px;
  font-size: 12px;
  color: #888;
}

.input-group {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.input-group input[type="text"] {
  flex-grow: 1;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.button-group {
  display: flex;
}

.button-group button {
  margin-left: 5px;
  padding: 8px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.add-button {
  background-color: #4CAF50;
  color: white;
}

.task-button {
  background-color: #cc3333;
  color: white;
}

.completed-task-button {
  background-color: #ddd;
  color: #333;
}

.centered-button-group {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.clear-button {
  padding: 8px 15px;
  background-color: #666;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.spacer {
  width: 10px;
}
</style>
