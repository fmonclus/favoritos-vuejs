<script setup>
// props como objeto
defineProps({
  title: String,
  id: Number,
  body: {
    type: String,
    default: "Sin descripcion",
  },
});

const emit = defineEmits(['fijarFavorito '])
</script>

<template>
  <div class="card">
    <div class="card-body">
      <h5 class="card-title">{{ id }} - {{ title }}</h5>
      <p>{{ body }}</p>

      <button
        @click="emit('fijarFavorito', title, id)"
        class="btn btn-outline-primary"
      >
        Favorito
      </button>
    </div>
  </div>
</template>

<style scoped>
</style>