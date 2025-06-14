<template>
  <div class="todo-app">
    <h1>📝 Список дел</h1>

    <input
      v-model="newTask"
      @keyup.enter="addTask"
      placeholder="Введите задачу"
    />

    <button @click="addTask">Добавить</button>

    <p v-if="tasks.length === 0">Список пуст!</p>

    <ul>
      <li
        v-for="(task, index) in filteredTasks"
        :key="index"
        :class="{ done: task.done }"
      >
        <span @click="toggleTask(index)">
          {{ index + 1 }}. {{ task.text }}
        </span>
        <button @click="removeTask(index)">Удалить</button>
      </li>
    </ul>

    <div class="filters">
      <label>
        <input type="checkbox" v-model="showOnlyUndone" />
        Показать только невыполненные
      </label>
    </div>

    <p>Всего задач: {{ totalTasks }} | Осталось: {{ remainingTasks }}</p>
  </div>
</template>

<script>
export default {
  name: "TodoApp",
  data() {
    return {
      newTask: "",
      tasks: [],
      showOnlyUndone: false,
    };
  },
  methods: {
    addTask() {
      const text = this.newTask.trim();
      if (text) {
        this.tasks.push({ text, done: false });
        this.newTask = "";
      }
    },
    toggleTask(index) {
      this.tasks[index].done = !this.tasks[index].done;
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
  },
  computed: {
    totalTasks() {
      return this.tasks.length;
    },
    remainingTasks() {
      return this.tasks.filter((t) => !t.done).length;
    },
    filteredTasks() {
      return this.showOnlyUndone
        ? this.tasks.filter((t) => !t.done)
        : this.tasks;
    },
  },
};
</script>

<style scoped>
.todo-app {
  max-width: 400px;
  margin: 20px auto;
  font-family: sans-serif;
}

input {
  width: 70%;
  padding: 5px;
}

button {
  margin-left: 5px;
}

.done {
  text-decoration: line-through;
  color: gray;
}

ul {
  list-style: none;
  padding-left: 0;
}

li {
  margin: 8px 0;
}

.filters {
  margin-top: 10px;
}
li span {
  cursor: pointer;
  transition: color 0.2s;
}

li span:hover {
  color: #007bff;
}
</style>

