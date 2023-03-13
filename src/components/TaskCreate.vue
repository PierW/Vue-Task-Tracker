<script setup>
import { reactive } from 'vue';
import AppButton from '@/components/AppButton.vue';

let taskInput = reactive({
    value : '',
    err: '',
    isValid: true,

});

const emit = defineEmits(['create-task']);

// Metodo al Click sul bottone
const createTask = () => {
    taskInput.isValid = true;
    if (!taskInput.value) {
        taskInput.isValid = false;
        taskInput.err = "Compila tutti i campi!"
        return;
    }
    emit("create-task", taskInput.value);
    taskInput.value = "";
}

// Metodo al keyup nell'input
const checkValueLength = () => {
    if (taskInput.value && taskInput.isValid === false) {
        taskInput.isValid = true;
    }
    return;
}

</script>

<template>
    <div class="input-wrap" :class="{'input-err' : !taskInput.isValid}">
        <input type="text" @keyup="checkValueLength" v-model="taskInput.value" placeholder="Inersci del testo">
        <AppButton @click="createTask()"/>
    </div>
    <p class="err-msg" v-show="!taskInput.isValid">{{ taskInput.err }}</p>
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;
  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }
  &.input-err {
    border-color: red;
  }
  input {
    width: 100%;
    padding: 8px 6px;
    border: none;
    &:focus {
      outline: none;
    }
  }
}
.err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}

</style>