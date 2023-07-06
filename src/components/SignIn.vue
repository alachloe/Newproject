<template>
  <article class="auth-article card">
    <div v-if="errorMsg" class="error">
      {{ errorMsg }}
    </div>
    <form class="register flex-column" @submit.prevent="signIn">
      <h3 class="mb-10">Sign In</h3>
      <div class="w-100 flex-column">
        <input
          v-model="email"
          class="form-input"
          type="email"
          name="email"
          required
          placeholder="Email"
        /><br />
        <input
          v-model="password"
          class="form-input"
          type="password"
          name="password"
          required
          placeholder="Password"
        /><br />
        <button type="submit" class="button-dark">Log In</button>
      </div>
    </form>
  </article>
</template>

<script setup>
import { useRouter } from 'vue-router';
import { useUserStore } from '../stores/user';
import { ref } from 'vue';

const router = useRouter();
const email = ref('');
const password = ref('');
const errorMsg = ref(null);
const userStore = useUserStore();

async function signIn() {
  if (password.value && email.value) {
    try {
      await userStore.signIn(email.value, password.value);
      router.push('/');
    } catch (e) {
      errorMsg.value = e.message;
    }
  } else {
    errorMsg.value = 'Please enter valid login details';
  }
}
</script>