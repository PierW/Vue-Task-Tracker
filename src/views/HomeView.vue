<script setup>
import TaskCreate from '../components/TaskCreate.vue';
import TaskItem from '@/components/TaskItem.vue';
import NotoV1SadButRelievedFace from '~icons/noto-v1/sad-but-relieved-face';
import { ref } from 'vue';
import { uid } from 'uid';

const tasksList = ref([]);

const createTask = (task) => {
  tasksList.value.push({
    id: uid(),
    content: task,
    isCompleted: null,
    isEditing: null
  })
}

const toggleTaskStatus = (taskIndex) => {
  console.log(taskIndex);
}

</script>

<template>
  <main>
    <h1>Crea Task</h1>
    <TaskCreate @create-task="createTask"/>
    <ul class="task-list" v-if="tasksList.length > 0">
      <TaskItem v-for="(task, index) in tasksList" :key="task.id" :task="task" :index="index" @toggle-task-status="toggleTaskStatus"/>
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
