<script setup>
import TaskCreate from '../components/TaskCreate.vue';
import TaskItem from '@/components/TaskItem.vue';
import NotoV1SadButRelievedFace from '~icons/noto-v1/sad-but-relieved-face';
import NotoV1PartyPopper from '~icons/noto-v1/party-popper';
import { ref, onMounted, watch, computed } from 'vue';
import { uid } from 'uid';

const tasksList = ref([]);

onMounted(() => {
  fetchTaskList();
});

watch(
  tasksList,
  () => {
    // TODO: Sostituire questa condizione creando il componente TaskList mettendo l'ul e rimuovere localStorage al onBeforeUnmount()
    if(!tasksList.value.length){
      localStorage.removeItem('tasksList');
      return;
    }
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

const tasksCompleted = computed(() =>{
  return tasksList.value.every((task) => task.isCompleted);
});

const toggleTaskStatus = (taskIndex) => {
  tasksList.value[taskIndex].isCompleted = !tasksList.value[taskIndex].isCompleted;
}

const editTask = (taskIndex) => {
  tasksList.value[taskIndex].isEditing = !tasksList.value[taskIndex].isEditing;
}

const updateValue = (taskIndex, newTaskValue) => {
  tasksList.value[taskIndex].content = newTaskValue;
}

const deleteTask = (taskId) => {
  tasksList.value = tasksList.value.filter((task) => task.id !== taskId );
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
      <span>Non hai Task da completare! Creane uno!</span>
    </p>
    <p v-if="tasksCompleted && tasksList.length > 0" class="task-msg">
      <NotoV1PartyPopper width="22" height="100%"/>
      <span>Hai completato tutti i task!</span>
    </p>
  </main>
</template>

