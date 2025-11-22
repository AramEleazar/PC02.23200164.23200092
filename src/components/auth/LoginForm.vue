<template>
  <div class="login-page q-pa-md q-flex q-justify-center q-items-center">
    <q-card class="login-card q-pa-lg" flat bordered>
      <div class="brand q-mb-md">
        <img src="/icons/favicon-96x96.png" alt="logo" />
        <div class="title">Digimon Explorer</div>
        <div class="subtitle">Inicia sesión para explorar Digimons</div>
      </div>

      <q-form @submit.prevent="login">
        <q-input filled v-model="email" label="Correo electrónico" type="email" dense autofocus />
        <q-input filled v-model="password" label="Contraseña" type="password" dense class="q-mt-sm" />

        <div class="q-mt-md q-mb-sm">
          <q-btn type="submit" label="Iniciar sesión" color="primary" unelevated class="full-width" />
        </div>

        <div class="q-mt-sm q-gutter-sm row items-center">
          <div class="col">
            <q-btn flat icon="google" label="Google" />
          </div>
          <div class="col">
            <q-btn flat icon="facebook" label="Facebook" />
          </div>
        </div>

        <div class="q-mt-md text-center">
          <router-link to="/register">¿No tienes cuenta? Regístrate</router-link>
        </div>
      </q-form>
    </q-card>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'LoginForm',
  data() {
    return {
      email: '',
      password: ''
    }
  },
  methods: {
    async login () {
      // Basic validation
      if (!this.email || !this.password) {
        this.$q.notify({ type: 'negative', message: 'Ingresa correo y contraseña' })
        return
      }

      try {
        const payload = { email: this.email, password: this.password }
        const res = await axios.post('https://storedb-api.onrender.com/node-api/users/signin', payload, {
          headers: { 'Content-Type': 'application/json', 'accept': '*/*' }
        })

        // Expect token in response.data.token or response.data
        const token = res.data && (res.data.token || res.data.accessToken || res.data)
        if (!token) {
          throw new Error('Token no encontrado en la respuesta')
        }

        // Store token
        localStorage.setItem('token', typeof token === 'string' ? token : JSON.stringify(token))

        this.$q.notify({ type: 'positive', message: 'Inicio de sesión exitoso' })
        this.$router.push('/digimons')
      } catch (err) {
        console.error('Login error', err)
        const msg = err.response && err.response.data && err.response.data.message
          ? err.response.data.message
          : err.message || 'Error al iniciar sesión'
        this.$q.notify({ type: 'negative', message: msg })
      }
    }
  }
}
</script>

<style scoped>
.login-page {
  min-height: calc(100vh - 56px);
  background: linear-gradient(135deg, #0f172a 0%, #1e293b 50%, #0f172a 100%);
}
.login-card {
  width: 380px;
  background: rgba(255,255,255,0.06);
  backdrop-filter: blur(6px);
  color: #fff;
}
.brand {
  text-align: center;
}
.brand img { height: 64px; }
.title {
  font-size: 20px;
  font-weight: 700;
  margin-top: 8px;
}
.subtitle {
  font-size: 12px;
  color: #cbd5e1;
}
.full-width { width: 100%; }
</style>
