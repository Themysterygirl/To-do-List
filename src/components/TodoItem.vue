<template>
  <div>
    <input type="checkbox" v-model="task.completed" @change="toggleTask">
    <span :class="{ completed: task.completed }">{{ task.title }}</span>
    <button @click="deleteTask">Delete</button>
  </div>
</template>

<script>
export default {
  props: ['task'],
  methods: {
    toggleTask() {
      this.$emit('toggle', this.task);
    },
    deleteTask() {
      this.$emit('delete', this.task);
    },
  },
};
</script>

<style scoped>
.completed {
  text-decoration: line-through;
  color: gray;
}

button {
  margin-left: 10px;
  padding: 5px 10px;
  background-color: #ff3333;
  color: white;
  border: none;
  cursor: pointer;
}
</style>
