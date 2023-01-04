<script setup>
import { ref } from "vue";

import BlogPost from "./components/BlogPost.vue"
import PaginatePost from "./components/PaginatePost.vue"
import LoadingSpinner from "./components/LoadingSpinner.vue"

const posts = ref([])
const postXPagina = 10
const inicio = ref(0)
const fin = ref(postXPagina)
const loading = ref(true)
 
const favorito = ref('')

const cambiarFavorito = (title) => {
  favorito.value = title
};

const next = () => {
  inicio.value = inicio.value + postXPagina
  fin.value = fin.value + postXPagina
}

const prev = () => {
  inicio.value = inicio.value - postXPagina
  fin.value = fin.value - postXPagina
}

fetch('https://jsonplaceholder.typicode.com/posts')
.then(respuesta => respuesta.json())
.then(data => posts.value = data )
.finally(() => loading.value = false)
</script>

<template>
  <LoadingSpinner v-if="loading" />
  <div class="container" v-else>

    <h1>APP</h1>
    <h2>Mi Post Favorito:{{ favorito }}</h2>



    <PaginatePost class="mb-2"
    @next = 'next'
    @prev = 'prev' 
    :inicio = 'inicio'
    :fin="fin"
  

    />
    <BlogPost v-for="post in posts.slice(inicio,fin)" :key="post.id" :title="post.title" :id="post.id" :body="post.body"
    class="mb-2"
    @cambiarFavoritoNombre = "cambiarFavorito"
    ></BlogPost>

  </div>
</template>