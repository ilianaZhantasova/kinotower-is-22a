<script lang="ts" setup>
const authStore = useAuthStore();
const genderStore = useGenderStore();
const router = useRouter();
const email = ref('');
const password = ref('');
const errorMsg = ref('');
const fio = ref('');
const birthday = ref('');
const gender = ref(1);

const register = async () => {
  try{
    await authStore.signUp({
      fio:fio.value,
      email:email.value,
      password:password.value,
      birthday:birthday.value,
      gender_id:gender.value,
    });
    router.push('/');
  }catch(error){
    errorMsg.value = error.message;
  }
};
</script>



<template>
<main class="form-signin w-100 m-auto">
  <form @submit.prevent="register">
    <img class="mb-4" src="/logo.svg" alt="" width="72" height="57">
    <span class = "fs-2">inotower</span>
    <h1 class="h3 mb-3 fw-normal">Please sign up</h1>
    <div class="form-floating mt-2">
      <input type="text" v-model="fio" class="form-control" id="fio">
      <label for="fio">Username</label>
    </div>
    <div class="form-floating mt-2">
      <input type="email" v-model="email" class="form-control" id="floatingInput" placeholder="name@example.com">
      <label for="floatingInput">Email address</label>
    </div>
    <div class="form-floating">
      <input type="password" v-model="password" class="form-control" id="floatingPassword" placeholder="Password">
      <label for="floatingPassword">Password</label>
    </div>
    <div class="form-floating">
      <input type="date" v-model="birthday" class="form-control" id="birthday">
      <label for="fio">Birthday</label>
    </div>
    <div class="form-floating">
    <select class="form select" v-model="gender">
    <option value="null">Select gender...</option>
    <option
    v-for="gender in genderStore.genders"
    :key="gender.id"
    :value="gender.id">{{ gender.name }}</option>
    </select>
    </div>
    <button class="btn btn-primary w-100 py-2 mt-2" type="submit">Sign up</button>
    <p class="mt-5 mb-3 text-body-secondary">© 2017–2024</p>
  </form>
  <div v-if="errorMsg" class="alert alert-danger" role="alert">
  {{ errorMsg }}
</div>
</main>
</template>

<style>

</style>