<script setup>
import PhCheckCircleBold from '~icons/ph/check-circle-bold'
import PhNotePencilBold from '~icons/ph/note-pencil-bold'
import PhTrashBold from '~icons/ph/trash-bold'

defineProps({
    task: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
      required: true
    }
});

</script>

<template>

    <li class="task task__item">
        <input class="task__checkbox" type="checkbox" :name="task.id" :checked="task.isCompleted" @input="$emit('toggle-task-status', index)"/>
        <div class="task__content">
            <input v-if="task.isEditing" type="text" :value="task.content" @input="$emit('update-value', index, $event.target.value)"/>
            <span v-else :class="{'task--completed': task.isCompleted}">
                {{ task.content }}
            </span>
        </div>
        <div class="task__actions">
            <PhCheckCircleBold v-if="task.isEditing" class="icon" color="#41b080" width="22" height="100%" @click="$emit('edit-task', index)"/>
            <PhNotePencilBold v-else class="icon" color="#41b080" width="22" height="100%" @click="$emit('edit-task', index)" />
            <PhTrashBold class="icon" color="#f95e5e" width="22" height="100%" @click="$emit('delete-task', task.id)"/>
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
  .task{
    &--completed{
      text-decoration: line-through;
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