<template>
    <div>
      <form @submit.prevent="addTask">
        <input v-model="newTask" />
        <button>Add New Task</button>
      </form>
  
      <button @click="createTask">Register Task</button>
  
      <div v-for="task in filteredTasks" :key="task.id">
        <input type="checkbox" v-model="task.done" />
        <span :class="{ done: task.done }">{{ task.text }}</span>
        <button @click="removeTask(task)">Delete</button>
      </div>
  
      <button @click="hideCompleted = !hideCompleted">
        {{ hideCompleted ? 'Show all tasks' : 'Hide completed' }}
      </button>
  
      <button @click="logout">Log Out</button>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue';
  import { useRouter } from 'vue-router';
  import { supabase } from '../supabase';
  import { useTaskStore } from '../stores/task';
  
  const taskStore = useTaskStore();
  
  let id = 0;
  const newTask = ref('');
  
  const hideCompleted = ref(false);
  
  const tasks = ref([]);
  const filteredTasks = computed(() =>
    hideCompleted.value ? tasks.value.filter((t) => !t.done) : tasks.value
  );
  
  function addTask() {
    tasks.value.push({
      id: id++,
      text: newTask.value,
      done: false
    });
  
    newTask.value = '';
  }
  
  function removeTask(task) {
    const index = tasks.value.indexOf(task);
    if (index !== -1) {
      tasks.value.splice(index, 1);
    }
  }
  
  const router = useRouter();
  
  async function logout() {
    const { error } = await supabase.auth.signOut();
    router.push('/auth');
  }
  </script>
  