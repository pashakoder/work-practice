<script setup>
import { inject, ref, reactive } from 'vue';
import axios from 'axios';

const data = inject('data');

const popupActive = ref(false);
const isScrollBlocked = ref(false);

let user = reactive({
  name: '',
  pswd: '',
  error: ''
});

const validateInput = () => {
  let isValid = true;

  // Проверка имени пользователя
  if (!user.name || /[<>\\*+%/ ]/.test(user.name)) {
    user.error = 'Имя пользователя не должно содержать спец символы.';
    isValid = false;
  }

  // Проверка пароля
  if (!user.pswd || /[<>\\*+%/ ]/.test(user.pswd)) {
    user.error = 'Имя пользователя не должно содержать спец символы.';
    isValid = false;
  }

  return isValid;
};

const handleLogin = async () => {
  if (!validateInput()) {
    return;
  }

  try {
    const response = await axios.post('http://localhost:3000/login', {
      username: user.name,
      password: user.pswd
    });
    if (response.data.isAdmin) {
      data.user.value.isAdmin = true;
      data.user.value.passAdmin = user.pswd;
    }
    data.user.value.isLogged = true;
    data.user.value.name = user.name;
    closePopup();
    user = { name: '', pswd: '', error: '' };
  } catch (error) {
    user.error = error;
    console.error('Login failed:', error);
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

const openPopup = () => {
  popupActive.value = true;
  toggleScroll();
};

const closePopup = () => {
  popupActive.value = false;
  toggleScroll();
};
</script>

<template>
  <button class="login__text" @click="openPopup">Войти</button>
  <Teleport to="body">
    <div v-show="popupActive" class="popupLogin-bg" @click="closePopup">
      <div class="popupLogin" @click.stop>
        <h1>Login</h1>
        <div class="popupLogin__check">
          <input type="text" v-model="user.name" placeholder="Имя пользователя" />
          <input type="password" v-model="user.pswd" placeholder="Пароль" />
          <p v-if="user.error != ''" class="error">{{ user.error }}</p>
        </div>
        <button @click="handleLogin">Войти</button>
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

.popupLogin {
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

.popupLogin__check {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 20px;
}

.popupLogin > * {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
