<script setup>
import { onMounted, ref, computed } from "vue";

import BlogPost from "./components/BlogPost.vue";
import PaginatePost from "./components/PaginatePost.vue";
import LoadingSpinner from "./components/LoadingSpinner.vue";

const miFavorito = ref("");
const posts = ref([]);
const postXpage = 10;
const inicio = ref(0);
const fin = ref(postXpage);
const loading = ref(false);

onMounted(async () => {
  loading.value = true;
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/posts");
    posts.value = await res.json();
  } catch (error) {
    console.log(error);
  } finally {
    setTimeout(() => (loading.value = false), 1500);
  }
});

const fijarFavorito = (title, id) => {
  miFavorito.value =
    id + " - " + (title.length > 30 ? title.substring(0, 30) + "..." : title);
};

const next = () => {
  inicio.value = inicio.value + postXpage;
  fin.value = fin.value + postXpage;
};

const top = () => {
  window.scroll({ top: 0, left: 0, behavior: "smooth" });
};

const prev = () => {
  inicio.value = inicio.value - postXpage;
  fin.value = fin.value - postXpage;
};

const maxLength = computed(() => posts.value.length);

const paginatePage = computed(() => posts.value.slice(inicio.value, fin.value));
</script>

<template>
  <LoadingSpinner v-if="loading" />
  <div class="container" v-else>
    <h2>{{ miFavorito || "Sin post favoritos" }}</h2>

    <BlogPost
      v-for="post in paginatePage"
      :key="post.title"
      :title="post.title"
      :id="post.id"
      :body="post.body"
      class="mb-2"
      @fijarFavorito="fijarFavorito"
    >
    </BlogPost>

    <!-- <p>{{ inicio }} - {{ fin }}</p> -->
    <PaginatePost
      @next="next"
      @prev="prev"
      @top="top"
      :inicio="inicio"
      :fin="fin"
      :maxLength="maxLength"
      class="mb-2"
    ></PaginatePost>
  </div>
</template>


<style scoped>
.h2,
h2 {
  font-size: calc(1.325rem + 0.9vw);
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  color: #0d6efd;
}

.container {
  padding-top: 2em;
}
</style>
