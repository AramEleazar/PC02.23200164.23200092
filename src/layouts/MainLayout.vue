<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn flat dense round icon="menu" aria-label="Menu" @click="toggleLeftDrawer" />

        <q-toolbar-title>
          <img src="/icons/favicon-32x32.png" alt="logo" style="height:28px; margin-right:8px; vertical-align:middle" />
          Digimon Explorer
        </q-toolbar-title>

        <q-input dense square debounce="300" placeholder="Buscar Digimon..." v-model="search" @input="onSearch" style="max-width:320px" />

        <div style="margin-left: 12px">v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      <q-list>
        <q-item-label header> Navegación </q-item-label>

        <q-item clickable v-ripple to="/">
          <q-item-section avatar>
            <q-icon name="home" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Inicio</q-item-label>
            <q-item-caption>Resumen</q-item-caption>
          </q-item-section>
        </q-item>

        <q-item clickable v-ripple to="/digimons">
          <q-item-section avatar>
            <q-icon name="pets" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Digimons</q-item-label>
            <q-item-caption>Explora por nivel</q-item-caption>
          </q-item-section>
        </q-item>

        <q-item clickable v-ripple to="/login">
          <q-item-section avatar>
            <q-icon name="login" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Iniciar sesión</q-item-label>
            <q-item-caption>Accede a tu cuenta</q-item-caption>
          </q-item-section>
        </q-item>

      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view :searchQuery="search" />
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { ref } from 'vue'
import EssentialLink from 'components/EssentialLink.vue'
import { useRouter } from 'vue-router'

const linksList = [
  {
    title: 'Docs',
    caption: 'quasar.dev',
    icon: 'school',
    link: 'https://quasar.dev',
  },
  {
    title: 'Github',
    caption: 'github.com/quasarframework',
    icon: 'code',
    link: 'https://github.com/quasarframework',
  },
  {
    title: 'Discord Chat Channel',
    caption: 'chat.quasar.dev',
    icon: 'chat',
    link: 'https://chat.quasar.dev',
  },
  {
    title: 'Forum',
    caption: 'forum.quasar.dev',
    icon: 'record_voice_over',
    link: 'https://forum.quasar.dev',
  },
  {
    title: 'Twitter',
    caption: '@quasarframework',
    icon: 'rss_feed',
    link: 'https://twitter.quasar.dev',
  },
  {
    title: 'Facebook',
    caption: '@QuasarFramework',
    icon: 'public',
    link: 'https://facebook.quasar.dev',
  },
  {
    title: 'Quasar Awesome',
    caption: 'Community Quasar projects',
    icon: 'favorite',
    link: 'https://awesome.quasar.dev',
  },
]

const leftDrawerOpen = ref(false)
const search = ref('')
const router = useRouter()

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value
}

function onSearch() {
  // Emit a router navigation with query param to allow pages to pick up the search
  router.replace({ path: router.currentRoute.value.path, query: { search: search.value } })
}
</script>
