<script setup>
import TaskCreate from '../components/TaskCreate.vue';
import TaskItem from '@/components/TaskItem.vue';
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

</script>

<template>
  <main>
    <h1>Crea Task</h1>
    <TaskCreate @create-task="createTask"/>
    <ul class="todo-list">
      <TaskItem v-for="task in tasksList" :key="task.id" :task="task"/>
    </ul>
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
  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }
  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
