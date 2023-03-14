<script setup>
import TaskCreate from '../components/TaskCreate.vue';
import TaskList from '../components/TaskList.vue';
import { ref, onMounted, watch } from 'vue';
import { uid } from 'uid';

const tasksList = ref([]);

onMounted(() => {
  fetchTaskList();
});

watch(
  tasksList,
  () => {
    setTasksListLocalStorage();
  },
  {
    deep: true
  }
);

const fetchTaskList = () => {
  const res = JSON.parse(localStorage.getItem("tasksList"));
  if (res) {
    tasksList.value = res;
  }
}

const setTasksListLocalStorage = () => {
  localStorage.setItem("tasksList", JSON.stringify(tasksList.value));
}

const createTask = (task) => {
  tasksList.value.push({
    id: uid(),
    content: task,
    isCompleted: null,
    isEditing: null
  });
}

const updateTasksList = (newTasksList) => {
  tasksList.value = newTasksList;
}
</script>

<template>
  <main>
    <h1>Crea Task</h1>
    <TaskCreate @create-task="createTask"/>
    <!-- Inserita condizione per eliminare componente ed attivare il lifecycle hook beforeUnmount -->
    <TaskList v-if="tasksList.length" :tasksList="tasksList" @update:tasksList="updateTasksList" />
  </main>
</template>

