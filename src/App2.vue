<script setup lang="ts">
import { onMounted, ref } from 'vue';

  const message = ref("Hello World!");
  const status = ref('active');
  const tasks = ref(["braa", "bitch", "wtf"]);
  const link = ref('https://www.google.com/');

  const newTask = ref('');

  const handleState = () => {
    if(status.value === 'active') {
      status.value = 'pending'; // fix typo here
    } else if (status.value === 'pending') {
      status.value = 'enactive';
    } else {
      status.value = 'active';
    }
  };

  const addTask = () => {
    if(newTask.value.trim() !== '') {
      tasks.value.push(newTask.value);
      newTask.value = '';
    }
  };

  const deleteTask = (index:any) => {
    tasks.value.splice(index, 1);
  };

  onMounted(async () => {
    try {
      const res = await fetch('https://jsonplaceholder.typicode.com/todos');
      const data = await res.json();
      tasks.value = data.map((task: any) => task.title);
    } catch (e: any) {
      console.log("Error fetch tasks");
    }
  })
</script>

<template>
  <h1>{{ message }}</h1>
  <p v-if="status === 'active'">
    user is active
  </p>
  <p v-else-if="status === 'pending'">
    user is pending
  </p>
  <p v-else>
    user is enactive
  </p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask">
    <button type="submit">Submit</button>
  </form>

  <h3>Tasks: </h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="deleteTask(index)">x</button>
    </li>
  </ul>
  <a :href="link">google here click me!</a>

  <button @click="handleState">change state</button>
</template>

<style scoped>
</style>