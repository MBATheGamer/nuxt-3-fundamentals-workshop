<script lang="ts">
  import { defineNuxtComponent } from '#app';
  import type { Photo } from './types';

  export default defineNuxtComponent({
    data: () => ({
      photos: [] as Photo[],
    }),
    computed: {
      numberOfPhotos() {
        return this.photos.length;
      },
      evenAlbums() {
        return this.photos.filter(photo => photo.albumId % 2 === 0);
      },
      oddAlbums() {
        return this.photos.filter(photo => photo.albumId % 2 !== 0);
      }
    },
    methods: {
      fetchPhotos() {
        fetch("https://jsonplaceholder.typicode.com/photos/")
          .then(response => response.json())
          .then(json => {
            this.photos = json as Photo[];
          });
      }
    }
  })
</script>

<template>
  <div>
    <h1>Photo Gallery</h1>
    <button @click="fetchPhotos">Fetch data</button>
    <h2>Total photos is: {{ numberOfPhotos }}</h2>
    <h3>({{ oddAlbums.length }} odd albums | {{ evenAlbums.length }} even albums)</h3>
    <ul>
      <li v-for="photo in photos" :key="photo.id">
        <img :src="photo.url" :alt="photo.title">
      </li>
    </ul>
  </div>
</template>
