<template>
  <div class="login-container">
    <form class="login-box" @submit="login">
      <h3 class="title">Apimon</h3>

      <label class="label" for="username">Email</label>
      <input
        class="input"
        type="text"
        placeholder="example@peru.com"
        id="username"
        v-model="email"
      >

      <label class="label" for="password">Password</label>
      <input
        class="input"
        type="password"
        placeholder="Password"
        id="password"
        v-model="password"
      >

      <button class="btn" type="submit">Log In</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "LoginForm",
  data() {
    return {
      email: '',
      password: ''
    }
  },
  methods: {
    async login(event) {
      event.preventDefault();

      try {
        const endpointURL = "https://storedb-api.onrender.com/node-api/users/signin";

        const userData = {
          email: this.email,
          password: this.password
        };

        const response = await axios.post(endpointURL, userData);

        localStorage.setItem("token", response.data.token);

        this.$router.push('/products');

      } catch {
        this.$q.notify({
          type: 'negative',
          message: "Email o password incorrectos",
          position: 'bottom'
        });
      }
    }
  }
}
</script>

<style scoped>
/* Fondo naranja + azul estilo Agumon + Digivice vibes */
.login-container {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(145deg, #ff7a00, #003c8f);
  font-family: 'Trebuchet MS', sans-serif;
}

/* Caja con brillo y estilo digital */
.login-box {
  background: rgba(255, 255, 255, 0.15);
  padding: 40px;
  border-radius: 15px;
  backdrop-filter: blur(12px);
  width: 330px;
  text-align: center;
  border: 2px solid #ffb200;
  box-shadow: 0 0 20px rgba(255, 145, 0, 0.6),
              0 0 30px rgba(0, 150, 255, 0.4);
}

/* Título digital con colores Agumon */
.title {
  color: #ffcf40;
  font-size: 32px;
  margin-bottom: 25px;
  text-shadow: 0 0 8px #ffa600,
               0 0 15px #005fff;
}

/* Labels */
.label {
  color: #ffe9c0;
  font-weight: bold;
  display: block;
  margin-top: 15px;
  text-align: left;
}

/* Inputs */
.input {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  border-radius: 8px;
  border: none;
  outline: none;
  font-size: 15px;
  background: rgba(255, 255, 255, 0.9);
}

/* Botón naranja fuego estilo Agumon */
.btn {
  margin-top: 20px;
  width: 100%;
  padding: 12px;
  font-size: 16px;
  border-radius: 8px;
  border: none;
  background: #ff8c00;
  color: #002f5c;
  font-weight: bold;
  cursor: pointer;
  transition: 0.25s;
}

.btn:hover {
  background: #ffaa2c;
  color: #001f3d;
  box-shadow: 0 0 12px #ff8c00,
              0 0 20px #008cff;
}
</style>
