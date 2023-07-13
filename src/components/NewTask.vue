<template>
    <div class="container">
      <h2 class="header-title">Add a new Task</h2>
      <div>
        <p class="date"><strong> {{ formattedDate }}</strong></p>
      </div>
      <div v-if="showErrorMessage">
        <p class="error-text">{{ errorMessage }}</p>
      </div>
      <form @submit.prevent="addTask">
        <input v-model="newTask" />
        <button>Add New Task</button>
      </form>
  
      <div v-for="task in filteredTasks" :key="task.id" class="checkbox-wrapper">
        <input type="checkbox" v-model="task.done" />
        <span :class="{ done: task.done }">{{ task.text }}</span>
        <button @click="removeTask(task)">Delete</button>
      </div>
  
      <button @click="hideCompleted = !hideCompleted" class="hide-button">
        {{ hideCompleted ? 'Show all tasks' : 'Hide completed' }}
      </button>
    </div>
  </template>
  
  
  <script setup>
  import { ref, computed, onMounted } from 'vue';
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
  
  const currentDate = ref(new Date());
  
  const updateCurrentDate = () => {
    currentDate.value = new Date();
  };
  
  onMounted(() => {
    updateCurrentDate();
    setInterval(updateCurrentDate, 24 * 60 * 60 * 1000); // Actualizar cada día (24 horas * 60 minutos * 60 segundos * 1000 milisegundos)
  });
  
  const formattedDate = ref('');
  
  onMounted(() => {
    const options = { day: 'numeric', month: 'long', year: 'numeric' };
    const formatter = new Intl.DateTimeFormat('en', options);
    formattedDate.value = formatter.format(currentDate.value);
  });
  
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
  
  <style scooped >
  
  .header-title{
    color:rgb(51, 134, 80);
    text-align: center;
    margin-bottom: 20px;
    text-shadow: 2px 2px 4px rgb(250, 250, 250);
  
    font-size: 75px;
  }
  .subtitle {
    color: white;
    text-align: center;
    margin-top: 50px;
    margin-bottom: 10px;
    font-size: 18px;
    font-family: Verdana, Geneva, Tahoma, sans-serif;
  }
  .date{
    color:rgb(34, 92, 54);
    font-family: Verdana, Geneva, Tahoma, sans-serif;
  }
  
  .subtitle {
    color: white;
    text-align: center;
    margin-bottom: 10px;
    font-size: 18px;
    font-family: Verdana, Geneva, Tahoma, sans-serif;
  }
  .date{
    color:rgb(70, 181, 109);
    font-family: Verdana, Geneva, Tahoma, sans-serif;
  }

  </style>
    