<script setup>
import TaskCreate from '../components/TaskCreate.vue';
import TaskItem from '@/components/TaskItem.vue';
import NotoV1SadButRelievedFace from '~icons/noto-v1/sad-but-relieved-face';
import { ref, onMounted } from 'vue';
import { uid } from 'uid';

const tasksList = ref([]);

onMounted(() => {
  fetchTaskList();
});

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
  setTasksListLocalStorage();
}

const toggleTaskStatus = (taskIndex) => {
  tasksList.value[taskIndex].isCompleted = !tasksList.value[taskIndex].isCompleted;
  setTasksListLocalStorage();
}

const editTask = (taskIndex) => {
  tasksList.value[taskIndex].isEditing = !tasksList.value[taskIndex].isEditing;
  setTasksListLocalStorage();
}

const updateValue = (taskIndex, newTaskValue) => {
  tasksList.value[taskIndex].content = newTaskValue;
  setTasksListLocalStorage();
}

const deleteTask = (taskId) => {
  tasksList.value = tasksList.value.filter((task) => task.id !== taskId );
  setTasksListLocalStorage();
}

</script>

<template>
  <main>
    <h1>Crea Task</h1>
    <TaskCreate @create-task="createTask"/>
    <ul class="task-list" v-if="tasksList.length > 0">
      <TaskItem v-for="(task, index) in tasksList" :key="task.id" :task="task" :index="index" @toggle-task-status="toggleTaskStatus" @edit-task="editTask" @update-value="updateValue" @delete-task="deleteTask"/>
    </ul>
    <p class="task-msg" v-else>
      <NotoV1SadButRelievedFace width="22" height="100%"/>
      <span>Non hai Task completati!</span>
    </p>
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;
  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
  .task-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }
  .task-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
