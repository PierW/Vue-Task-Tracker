<script>
import TaskItem from '@/components/TaskItem.vue';
import NotoV1SadButRelievedFace from '~icons/noto-v1/sad-but-relieved-face';
import NotoV1PartyPopper from '~icons/noto-v1/party-popper';
import { computed } from 'vue';

export default {
    components: {
        TaskItem,
        NotoV1SadButRelievedFace,
        NotoV1PartyPopper,
    },
    props: {
        tasksList: {
            type: Array,
            required: true,
        },
    },
    setup(props, { emit }) {
        const toggleTaskStatus = (taskIndex) => {
            props.tasksList[taskIndex].isCompleted = !props.tasksList[taskIndex].isCompleted;
        }

        const editTask = (taskIndex) => {
            props.tasksList[taskIndex].isEditing = !props.tasksList[taskIndex].isEditing;
        }

        const updateValue = (taskIndex, newTaskValue) => {
            props.tasksList[taskIndex].content = newTaskValue;
        }

        const deleteTask = (taskId) => {
            // Creo una nuova copia dell'array "tasksList" perchè la props è di sola lettura e devo modificare con l'emit quello di orgine nel componente padre.
            const newTasksList = props.tasksList.filter((task) => task.id !== taskId);
            /* 
            La sintassi update:tasksList significa che stiamo emettendo un evento personalizzato chiamato "update" 
            e che i dati associati a questo evento sono relativi alla prop tasksList.
            In altre parole, stiamo dicendo a Vue di aggiornare la prop tasksList nel componente padre
            quando l'evento update:tasksList viene emesso dal componente figlio. 
            */
            emit('update:tasksList', newTasksList);
        }

        const tasksCompleted = computed(() => {
            return props.tasksList.every((task) => task.isCompleted);
        });

        return {
            toggleTaskStatus,
            editTask,
            updateValue,
            deleteTask,
            tasksCompleted,
        }
    },
    beforeUnmount() {
        localStorage.removeItem('tasksList');
        console.log('Componente Eliminato');
    }

}
</script>

<template>
    <div class="task-list-container">
        <ul class="task-list" v-if="tasksList.length > 0">
            <TaskItem v-for="(task, index) in tasksList" :key="task.id" :task="task" :index="index"
                @toggle-task-status="toggleTaskStatus" @edit-task="editTask" @update-value="updateValue"
                @delete-task="deleteTask" />
        </ul>
        <p class="task-msg" v-else>
            <NotoV1SadButRelievedFace width="22" height="100%" />
            <span>Non hai Task da completare! Creane uno!</span>
        </p>
        <p v-if="tasksCompleted && tasksList.length > 0" class="task-msg">
            <NotoV1PartyPopper width="22" height="100%" />
            <span>Hai completato tutti i task!</span>
        </p>
    </div>
</template>