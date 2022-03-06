<template>
  <div class="users-form">
    <form method="post">
      <input class="input-box" type="text" v-model="userName" placeholder="user name">
      <br><input class="input-box" type="email" v-model="userEmail" placeholder="email" required>
      <br><input class="input-box" type="password" v-model="userPassword" placeholder="password" required>
    </form>
  </div>
  <slot :userName="userName" :userEmail="userEmail" :userPassword="userPassword" />
</template>

<script setup>
import { inject, watch } from 'vue'
import { ref } from 'vue'
const clearForm = inject('clearForm')
const userName = ref('')
const userPassword = ref('')
const userEmail = ref('')
watch(clearForm, () => {
  if (clearForm.value === false){
    userName.value = ''
    userPassword.value = ''
    userEmail.value = ''
  }
})
</script>

<style scoped>
form {
  margin-top: 50px;
}
.input-box {
  width: 350px;
  padding: 5px 20px;
  margin: 5px 0;
  display: inline-block;
  border: 3px solid blue;
  border-radius: 8px;
  box-sizing: border-box;
  font-size: 1.2rem;
}
</style>