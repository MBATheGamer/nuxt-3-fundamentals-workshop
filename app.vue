<script setup lang="ts">
  import { ref, computed } from "vue";
  import type { Photo } from "./types";

  const photos = ref<Photo[]>([]);

  const numberOfPhotos = computed(() => photos.value.length);

  const evenAlbums = computed(() => photos.value.filter(photo => photo.albumId % 2 === 0));

  const oddAlbums = computed(() => photos.value.filter(photo => photo.albumId % 2 !== 0));

  function fetchPhotos() {
    fetch("https://jsonplaceholder.typicode.com/photos/")
      .then(response => response.json())
      .then(json => {
        photos.value = json as Photo[];
      });
  }
</script>

<template>
  <div class="container">
    <h1>Photo Gallery</h1>
    <button @click="fetchPhotos">Fetch data</button>
    <h2>Total photos is: {{ numberOfPhotos }}</h2>
    <h3>({{ oddAlbums.length }} odd albums | {{ evenAlbums.length }} even albums)</h3>
    <ul class="photo-gallary-list">
      <li v-for="photo in photos" :key="photo.id">
        <img :src="photo.url" :alt="photo.title">
      </li>
    </ul>
  </div>
</template>

<style scoped lang="scss">
  @import './node_modules/bulma/bulma.sass';
  .photo-gallary-list {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
  }
</style>
