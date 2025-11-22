<template>
  <div>
    <div v-if="!digimons || digimons.length === 0" class="q-pa-md">
      <h3>No hay Digimons para mostrar</h3>
    </div>
    <div class="cards q-gutter-md q-pa-md row">
      <div v-for="digimon in digimons" :key="digimon.name" class="card col-12 col-sm-6 col-md-4">
        <div class="q-card q-pa-md">
          <img :src="digimon.img" :alt="digimon.name" class="digimon-image" />
          <h5 class="q-mt-sm">{{ digimon.name }}</h5>
          <p class="level">Nivel: {{ digimon.level }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DigimonList',
  props: {
    digimons: {
      type: Array,
      default: () => []
    }
  }
}
</script>

<style scoped>
.cards {
  display: flex;
  flex-wrap: wrap;
}
.card {
  padding: 8px;
}
.digimon-image {
  width: 100%;
  height: 180px;
  object-fit: contain;
  background: #fff;
  border-radius: 8px;
}
.q-card {
  background: #fff;
  border-radius: 8px;
  padding: 12px;
  text-align: center;
}
.level {
  color: #666;
}
</style>
