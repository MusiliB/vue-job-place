<script>
export default {
  data() {
    return {
      name: 'John Doe',
      status: 'active',
      tasks: [
        { id: 1, title: 'Task 1', completed: true },
        { id: 2, title: 'Task 2', completed: false },
        { id: 3, title: 'Task 3', completed: true },
      ],
      link: 'https://www.example.com'
    }
  },
  methods: {
    changeStatus() {
      if (this.status === 'active') {
        this.status = 'pending';
      } else if (this.status === 'pending') {
        this.status = 'inactive';
      } else {
        this.status = 'active';
      }
    }
  }
}
</script>

<template>
  <h1>{{ name }}</h1>

  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>Status is inactive</p>

  <ul>
    <li v-for="task in tasks" :key="task.id">
      <span :class="{ completed: task.completed }">{{ task.title }}</span>
    </li>
  </ul>

  <a :href="link">Visit Example</a>

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