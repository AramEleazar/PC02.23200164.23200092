<template>
  <div class="digimon-page">
    <div class="digimon-filter q-ml-md q-mr-xl">
      <DigimonFilter :levels="levels" :initialFilters="filters" @update-filters="onUpdateFilters" />
    </div>
    <div class="digimon-list q-ml-xl q-mr-md">
      <DigimonList :digimons="filteredDigimons" />
    </div>
  </div>

</template>

<style>
.digimon-page {
  display: flex;
  justify-content: space-between;
}

.digimon-filter {
  width: 25%;
}

.digimon-list {
  width: 75%;
}
</style>

<script>
import DigimonFilter from "src/components/digimon/DigimonFilter.vue";
import DigimonList from "src/components/digimon/DigimonList.vue";
export default {
  name: "DigimonPage",
  components: {
    DigimonFilter, DigimonList
  },
  data() {
    return {
      digimons: [],
      levels: [],
      filters: {
        name: '',
        level: 'All' // default to 'All' so filters work immediately
      }
    }
  },
  computed: {
    filteredDigimons() {
      return this.digimons.filter(d => {
        const matchesName = this.filters.name
          ? d.name.toLowerCase().includes(this.filters.name.toLowerCase())
          : true;
        const matchesLevel = this.filters.level && this.filters.level !== 'All'
          ? d.level === this.filters.level
          : true;
        return matchesName && matchesLevel;
      });
    }
  },
  mounted() {
    this.fetchDigimons();
  },
  methods: {
    async fetchDigimons() {
      try {
        const res = await fetch('https://digimon-api.vercel.app/api/digimon');
        const data = await res.json();
        this.digimons = Array.isArray(data) ? data : [];
        // extract unique levels
        const levelSet = new Set(this.digimons.map(d => d.level));
        this.levels = ['All', ...Array.from(levelSet).sort()];
        // ensure current filter level is valid
        if (!this.levels.includes(this.filters.level)) {
          this.filters.level = 'All';
        }
      } catch (err) {
        console.error('Error fetching digimons', err);
        this.$q.notify({ type: 'negative', message: 'Error al obtener Digimons' });
      }
    },
    onUpdateFilters(newFilters) {
      this.filters = { ...this.filters, ...newFilters };
    }
  }
};
</script>
