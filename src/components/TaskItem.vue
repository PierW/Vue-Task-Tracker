<script setup>
import { Icon } from '@iconify/vue';

    const tasks = defineProps({
        task: {
            type: Object,
            required: true
        }
    });
</script>

<template>

    <li class="task task__item">
        <input class="task__checkbox" type="checkbox" :name="task.id" :checked="task.isCompleted"/>
        <div class="task__content">
            <input v-if="task.isEditing" type="text" :value="task.content"/>
            <span v-else>
                {{ task.content }}
            </span>
        </div>
        <div class="task__actions">
            <Icon v-if="task.isEditing" class="icon" icon="ph:check-circle-bold" color="#41b080" width="22" />
            <Icon v-else class="icon" icon="ph:note-pencil-bold" color="#41b080" width="22" />
            <Icon class="icon" icon="ph:trash-bold" color="#f95e5e" width="22" />
        </div>
    </li>

</template>

<style lang="scss" scoped>
li {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 16px 10px;
  background-color: #f1f1f1;
  box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);
  &:hover {
    .task__actions {
      opacity: 1;
    }
  }
  input[type="checkbox"] {
    appearance: none;
    width: 20px;
    height: 20px;
    background-color: #fff;
    border-radius: 50%;
    box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
    &:checked {
      background-color: #41b080;
    }
  }
  .task__content {
    flex: 1;
    input[type="text"] {
      width: 100%;
      padding: 2px 6px;
      border: 2px solid #41b080;
    }
  }
  .task__actions {
    display: flex;
    gap: 6px;
    opacity: 0;
    transition: 150ms ease-in-out;
    .icon {
      cursor: pointer;
    }
  }
}
</style>