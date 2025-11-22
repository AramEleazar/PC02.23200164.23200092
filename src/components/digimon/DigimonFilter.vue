<template>
  <div>
    <h6>Filtros</h6>

    <div class="q-pa-sm">
      <q-input dense outlined v-model="localFilters.name" label="Filtrar por nombre" />
    </div>

    <div class="q-pa-sm">
      <q-select dense outlined v-model="localFilters.level" :options="levels" label="Nivel" />
    </div>
  </div>
</template>

<script>
export default {
  name: "DigimonFilter",
  props: {
    levels: {
      type: Array,
      default: () => ["All"],
    },
    initialFilters: {
      type: Object,
      default: () => ({ name: "", level: "All" }),
    },
  },
  data() {
    return {
      localFilters: { ...this.initialFilters },
    };
  },
  watch: {
    // keep local copy in sync if parent changes filters
    initialFilters: {
      handler(newVal) {
        this.localFilters = { ...newVal };
      },
      deep: true,
    },
    // if available levels change, ensure selected level is valid
    levels(newLevels) {
      if (this.localFilters.level && !newLevels.includes(this.localFilters.level)) {
        this.localFilters.level = 'All';
        this.emitFilters();
      }
    },
    // emit whenever the localFilters object changes (name or level)
    localFilters: {
      handler() {
        this.emitFilters();
      },
      deep: true,
    }
  },
  methods: {
    emitFilters() {
      this.$emit("update-filters", { ...this.localFilters });
    },
  },
};
</script>

<style scoped>
</style>
