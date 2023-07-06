<!-- COMPONENTE BOILERPLATE -->
 
<template>
  <div>
    <h1>Log In to ToDo App </h1>

    <div v-if="errorMsg">
      <p>{{ errorMsg }}</p>
    </div>

    <!-- Sign In -->
    <form @submit.prevent="login">
      <label>Email</label>
      <div>
        <input
          type="text"
          v-model="email"
          id="email"
          required="required"
          placeholder="Email"
          class=""
        />
      </div>
      <label>Password</label>
      <div>
        <input
          type="password"
          v-model="password"
          id="password"
          required="required"
          placeholder="Password"
          class=""
        />
      </div>

      <button type="submit" class="button-dark">Log In</button>
    </form>

    <p>
      Don't have an account?
      <PersonalRouter :route="route" :buttonText="buttonText" class="sign-up-link" />
    </p>
  </div>
</template>

<script setup>
import PersonalRouter from "./PersonalRouter.vue";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { ref } from "vue";

// Route Variables
const route = "/auth/signup";
const buttonText = "Sign Up";

// variables para conectarme al form (login)
const email = ref("");
const password = ref("");

// Router to push user once SignedIn to Home
const redirect = useRouter();

// Arrow function to Sign in user to supaBase
const login = async () => {
  try {
    await useUserStore().signIn(email.value, password.value);
    // Redirects user to the homeView
    redirect.push({ path: "/" });
  } catch (error) {
    alert(error);
  }
};

// Error message reactive variable
const errorMsg = ref(null);
</script>

<style></style>
