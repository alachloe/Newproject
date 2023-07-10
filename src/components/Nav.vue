<template>
  <nav>
    <!-- <router-link to="/">Home</router-link> | -->
    <!-- <router-link to="SignIn">Sign In</router-link> | -->
    <!-- <router-link to="SignUp>Sign Up</router-link> | -->
    <router-link to="/">Task Manager</router-link>
    <button @click="logout">Log Out</button>
  </nav>
  <router-view/>
</template>

<script setup>
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { ref } from 'vue';
import { supabase } from "../supabase";

const router = useRouter();
const getUser = useUserStore().user;
const userEmail = ref(getUser.email);

async function logout() {
  try {
    await supabase.auth.signOut();
    router.push('/auth');
  } catch (error) {
    console.log(error);
  }
}
</script>

<style scoped>
nav {
  display: flex;
  justify-content: center;
  gap: 2rem;
}
</style>
