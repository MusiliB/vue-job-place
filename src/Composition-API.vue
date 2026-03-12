<script setup>

import { onMounted, ref } from 'vue';


const name = ref('John Doe');
const status = ref('active');
const newTaskTitle = ref('');


const tasks = ref([
    { id: 1, title: 'Task 1', completed: true },
    { id: 2, title: 'Task 2', completed: false },
    { id: 3, title: 'Task 3', completed: true },
]);

function changeStatus() {
    if (status.value === 'active') {
        status.value = 'pending';
    } else if (status.value === 'pending') {
        status.value = 'inactive';
    } else {
        status.value = 'active';
    }
}


const addTask = () => {
    if (newTaskTitle.value.trim() === '') {
        return;
    }

    const newTask = {
        id: tasks.value.length + 1,
        title: newTaskTitle.value,
        completed: false,
    };

    tasks.value.push(newTask);
    newTaskTitle.value = '';
}

const deleteTask = (id) => {
    tasks.value = tasks.value.filter(task => task.id !== id);
}

onMounted(async () => {
    try {
        const response = await fetch('https://jsonplaceholder.typicode.com/todos?_limit=10');
        const data = await response.json();
        tasks.value = [...tasks.value, ...data.map(task => ({
            id: task.id,
            title: task.title,
            completed: task.completed,
        }))];

    } catch (error) {
        console.error('Error fetching tasks:', error);
    }
});

</script>

<template>
    <h1>{{ name }}</h1>

    <form @submit.prevent="addTask">
        <label for="new-task">Add task</label>
        <input type="text" id="new-task" v-model="newTaskTitle" />
        <button type="submit">Submit</button>
    </form>


    <p v-if="status === 'active'">User is active</p>
    <p v-else-if="status === 'pending'">User is pending</p>
    <p v-else>Status is inactive</p>

    <ul>
        <li v-for="task in tasks" :key="task.id">
            <span :class="{ completed: task.completed }">{{ task.title }}</span>
            <span>
                <button @click="deleteTask(task.id)">Delete</button>
            </span>
        </li>
    </ul>


    <button @click="changeStatus">Change Status</button>

</template>

<style scoped>
h1 {
    color: #42b983;
}

.completed {
    text-decoration: line-through;
    color: gray;
}
</style>