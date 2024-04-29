<script lang="ts" setup>
const authStore = useAuthStore();
const router = useRouter();
const email = ref('');
const password = ref('');
const errorMsg = ref('');
const login = async() => {
  try {
    if (email.value && password.value) {
      await authStore.signIn(email.value, password.value);
      router.push('/');
    } else {
      throw new Error('Please input email and password!');
    }

  } catch (error) { 
errorMsg.value = error.message;
  }
};
</script>

<template>
<main class="form-signin w-100 m-auto">
  <form @submit.prevent="login">
    <img class="mb-4" src="/logo.svg" alt="" width="72" height="57">
    <span class = "fs-2">inotower</span>
    <h1 class="h3 mb-3 fw-normal">Please sign in</h1>

    <div class="form-floating mt-2">
      <input type="email" v-model="email" class="form-control" id="floatingInput" placeholder="name@example.com">
      <label for="floatingInput">Email address</label>
    </div>
    <div class="form-floating">
      <input type="password" v-model="password" class="form-control" id="floatingPassword" placeholder="Password">
      <label for="floatingPassword">Password</label>
    </div>
    <button class="btn btn-primary w-100 py-2 mt-2" type="submit">Sign in</button>
    <p class="mt-5 mb-3 text-body-secondary">© 2017–2024</p>
  </form>
  <div v-if="errorMsg" class="alert alert-danger" role="alert">
  {{ errorMsg }}
</div>
</main>
</template>


<style>
.form-signin {
  max-width: 330px;
  padding: 1rem;
}

</style>