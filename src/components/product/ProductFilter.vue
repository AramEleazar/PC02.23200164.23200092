<template>
  <div>
    <h6>Filtros</h6>

    <div class="q-pa-sm">
      <q-input dense outlined v-model="localFilters.name" label="Filtrar por nombre" @input="emitFilters" />
    </div>

    <div class="q-pa-sm">
      <q-select dense outlined v-model="localFilters.level" :options="levels" label="Nivel" @input="emitFilters" />
    </div>
  </div>
</template>

<style scoped>
</style>

<script>
export default {
  name: "ProductFilter",
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
  methods: {
    emitFilters() {
      this.$emit("update-filters", { ...this.localFilters });
    },
  },
};
</script>
