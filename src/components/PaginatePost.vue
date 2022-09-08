<script setup>
defineProps(["inicio", "fin", "maxLength"]);
const emit = defineEmits(["next", "prev", "top"]);
</script>

<template>
    <div class="btn-group" role="group" aria-label="Basic example">
        <button
            type="button"
            class="btn btn-outline-primary"
            @click="emit('prev')"
            :disabled="inicio === 0"
        >
            Previo
        </button>
        <button
            type="button"
            class="btn btn-outline-primary"
            @click="emit('top')"            
        >
            Ir a principio
        </button>
        <button
            type="button"
            class="btn btn-outline-primary"
            @click="emit('next')"
            :disabled="fin >= maxLength"
        >
            Siguiente
        </button>
    </div>
</template>