<script setup>
import { ref, computed, onMounted, watch } from "vue";
const isLoading = ref(false);
const isUpdate = ref(false);
const isValid = ref(true);

const firstname = ref("");
const lastname = ref("");
const email = ref("");
const errors = ref("");

const fullname = computed(() => {
  return `${firstname.value} ${lastname.value}`;
});

const updateProfile = async () => {
  isLoading.value = true;
  //จำลองการส่ง API ออกมา
  await new Promise((resolve) => setTimeout(resolve, 2000));
  isLoading.value = false;
  isUpdate.value = true;
};

const validateName = (name) => {
  const re = /\d/;
  return !re.test(name);
};
const validateEmail = (email) => {
  return email.includes("@");
};

watch([firstname, lastname, email], () => {
  isValid.value = true;
  isUpdate.value = false;
  errors.value = {};

  if (!validateName(firstname.value)) {
    isValid.value = false;
    errors.value.firstname = "firstname incorrect";
  }

  if (!validateName(lastname.value)) {
    isValid.value = false;
    errors.value.lastname = "lastname incorrect";
  }

  if (!validateEmail(email.value)) {
    isValid.value = false;
    errors.value.email = "email incorrect";
  }
});

onMounted(() => {
  firstname.value = "Name";
  lastname.value = "Lastname";
  email.value = "example@gmail.com";
});
</script>

<template>
  <div class="container">
    <div>Fullname: {{ fullname }}</div>
    <div>email: {{ email }}</div>
    <div>
      <div>Firstname:</div>
      <input type="text" v-model="firstname" />
      <div class="error">{{ errors.firstname }}</div>
    </div>
    <div>
      <div>Lastname:</div>
      <input type="text" v-model="lastname" />
      <div class="error">{{ errors.lastname }}</div>
    </div>
    <div>
      <div>Email:</div>
      <input type="text" v-model="email" />
      <div class="error">{{ errors.email }}</div>
    </div>
    <div v-if="isLoading" class="loading">Loading...</div>
    <button :disabled="!isValid" class="button" @click="updateProfile()">
      Update Profile
    </button>
    <div v-if="isUpdate">Profile update completely</div>
  </div>
</template>

<style>
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  max-width: 320px;
  margin: 0 auto;
}

.container > div {
  width: 100%;
}

input {
  width: 100%;
}

.button {
  width: 100%;
  height: 24px;
  margin-top: 20px;
}

.loading {
  background-color: aliceblue;
  width: 100%;
  padding: 20px;
  box-sizing: border-box;
  margin: 10px 0;
}

.error {
  color: red;
}
</style>
