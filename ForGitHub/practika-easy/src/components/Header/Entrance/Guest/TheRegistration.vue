<script setup>
import { inject, ref, reactive } from 'vue';
import axios from 'axios';

const data = inject('data');

const popupActive = ref(false);
const isScrollBlocked = ref(false);

let newUser = reactive({
  name: '',
  pswd: '',
  pswd2: '',
  email: '',
  error: ''
});

const validateInput = () => {
  let isValid = true;

  // Проверка имени пользователя
  if (!newUser.name || !/[<>\\*+%/ ]/.test(newUser.name)) {
    newUser.error = 'Имя пользователя не должно содержать спец символы.';
    isValid = false;
  }

  // Проверка пароля
  if (!newUser.pswd || !/[<>\\*+%/ ]/.test(newUser.pswd)) {
    newUser.error = 'Пароль не должен содержать содержать спец символы.';
    isValid = false;
  }

  // Проверка повторного пароля
  if (newUser.pswd !== newUser.pswd2) {
    newUser.error = 'Пароли не совпадают.';
    isValid = false;
  }

  // Проверка email
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!emailPattern.test(newUser.email)) {
    newUser.error = 'Введите корректный email.';
    isValid = false;
  }

  return isValid;
};

const handleRegistration = async () => {
  if (!validateInput()) {
    return;
  }

  try {
    const response = await axios.post('http://localhost:3000/register', {
      username: newUser.name,
      password: newUser.pswd,
      email: newUser.email
    });
    console.log('Registration successful:', response.data);
    data.user.name = newUser.name;
    data.user.isLogged = true;
    Reset();
  } catch (error) {
    console.error('Registration failed:', error);
  }
};

const toggleScroll = () => {
  if (isScrollBlocked.value) {
    document.body.style.overflow = '';
  } else {
    document.body.style.overflow = 'hidden';
  }
  isScrollBlocked.value = !isScrollBlocked.value;
};

const Reset = () => {
  popupActive.value = false;
  newUser.name = '';
  newUser.pswd = '';
  newUser.pswd2 = '';
  newUser.email = '';
  newUser.error = '';
  toggleScroll();
};
</script>

<template>
  <button
    class="login__text"
    @click="
      popupActive = true;
      toggleScroll();
    "
  >
    Зарегистрироваться
  </button>
  <Teleport to="body">
    <div
      v-show="popupActive"
      class="popupRegistration-bg"
      @click="
        popupActive = false;
        toggleScroll();
      "
    >
      <div class="popupRegistration" @click.stop>
        <h1>Регистрация</h1>
        <div class="popupRegistration__check">
          <!-- prettier-ignore -->
          <input type="text" v-model="newUser.name" placeholder="Имя пользователя" />
          <input type="password" v-model="newUser.pswd" placeholder="Пароль" />
          <input type="password" v-model="newUser.pswd2" placeholder="Пароль для подтверждения" />
          <input type="email" v-model="newUser.email" placeholder="почта" />
          <p v-if="newUser.error != ''" class="error">{{ newUser.error }}</p>
        </div>
        <button @click="handleRegistration">Создать аккаунт</button>
      </div>
    </div>
  </Teleport>
</template>

<style scoped>
.error {
  color: red;
  font-size: 0.9em;
}

.login__text {
  color: #1e3072;
  font-size: 20px;
}

.popupRegistration {
  width: 40%;
  display: flex;
  flex-direction: column;
  align-self: center;
  overflow: hidden;
  background-color: rgb(255, 255, 255);
  border-radius: 2%;
  opacity: 1;
  padding: 30px;
  gap: 30px;
}

.popupRegistration__check {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 20px;
}

.popupRegistration > * {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
