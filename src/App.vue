<script setup>
import Todo from "./components/Todo.vue";
import { onMounted, ref } from "vue";
// const tasks = localStorage.getItem("tasks")
//   ? JSON.parse(localStorage.getItem("tasks"))
//   : ref([]);
const tasks = ref(
  localStorage.getItem("tasks")
    ? JSON.parse(localStorage.getItem("tasks"))
    : ref([])
);
const currentMode = ref(
  localStorage.getItem("theme") ? localStorage.getItem("theme") : "light"
);
const taskName = ref("");
const addTask = () => {
  if (taskName) {
    const newTask = {
      id: Math.random(),
      name: taskName.value,
      completed: false,
    };
    tasks.value.push(newTask);
    taskName.value = "";
    localStorage.setItem("tasks", JSON.stringify(tasks.value));
  }
};
const deleteTask = (id) => {
  tasks.value = tasks.value.filter((task) => task.id !== id);
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
};

const completedMark = (id) => {
  tasks.value = tasks.value.map((task) => {
    if (task.id === id) {
      task.completed = true;
      localStorage.setItem("tasks", JSON.stringify(tasks.value));
      location.reload();
    }
  });
};
const switchMode = () => {
  const currentModeFunc = localStorage.getItem("theme");
  if (currentModeFunc !== "dark") {
    localStorage.setItem("theme", "dark");
    currentMode.value = localStorage.getItem("theme");
    document.body.classList.add("dark");
  } else {
    localStorage.setItem("theme", "light");
    currentMode.value = localStorage.getItem("theme");
    document.body.classList.remove("dark");
  }
};
const getMode = () => {
  if (localStorage.getItem("theme") === "dark") {
    document.body.classList.add("dark");
  } else {
    document.body.classList.remove("dark");
  }
};
onMounted(getMode)
</script>

<template>
  <div>
    <button
      @click="switchMode"
      class="py-2 px-3 bg-blue-400 text-white rounded-md m-2"
    >
      current mode : {{ currentMode }}
    </button>
  </div>
  <div
    className="max-w-md mx-auto mt-10 p-6  bg-white rounded-lg shadow-lg dark:bg-slate-800 "
  >
    <h1 className="text-2xl font-bold mb-4 text-gray-800 dark:text-white">
      Todo List
    </h1>
    <div className="flex mb-4">
      <input
        type="text"
        v-model="taskName"
        placeholder="Add a new todo"
        class="flex-grow mr-2 pl-2 border-2 border-blue-400 rounded-md"
      />
      <button
        @click="addTask"
        class="font-semibold text-white py-2 px-3 rounded-md bg-blue-400 hover:bg-blue-500 transition duration-300"
      >
        إضافة
      </button>
    </div>
    <div class="flex flex-col gap-2" v-for="(task, index) in tasks">
      <Todo
        :id="task.id"
        :deleteTask="deleteTask"
        :completedMark="completedMark"
        :key="index"
        :taskName="task.name"
        :completed="task.completed"
      />
    </div>
  </div>
</template>

<style scoped></style>
