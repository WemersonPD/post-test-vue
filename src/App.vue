<template>
  <div id="app">
    <div class="d-flex justify-content-center mb-5">
      <input
        type="Search"
        placeholder="Type a title..."
        v-model="searchTitle"
        class="input-search"
      />
    </div>

    <div class="container">
      <div v-for="post in allPostsSearch()" :key="post.id" class="post-card">
        <img :src="post.imageUrl" :alt="post.title" />
        <h3>{{ post.title }}</h3>
        <p>{{ post.body }}</p>
      </div>
    </div>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      posts: [],
      searchTitle: "",
    };
  },
  methods: {
    async getAllPosts() {
      const postsResponse = await axios.get(
        "https://jsonplaceholder.typicode.com/posts"
      );
      return postsResponse.data;
    },

    async getAllPhotos() {
      const phonesResponse = await axios.get(
        "https://jsonplaceholder.typicode.com/photos"
      );
      return phonesResponse.data;
    },

    async getAllPostsAndPhotos() {
      const posts = await this.getAllPosts();
      const photos = await this.getAllPhotos();

      const postsWithPhotos = posts.map((post, index) => {
        post.imageUrl = photos[index].url;
        return post;
      });

      this.posts = postsWithPhotos;

      // const [posts, photos] = await Promise.all([
      //   this.getAllPosts(),
      //   this.getAllPhotos(),
      // ]);

      // const currentPhotos = photos.slice(0, 10);

      // Irá retorna um array de photos, ou seja, corretamente.
      // const postFormatted = await Promise.all(
      //   currentPhotos.map(async (photo) => {
      //     const album = await axios.get(
      //       `https://jsonplaceholder.typicode.com/albums/${photo.albumId}`
      //     );

      //     photo.album = album.data;

      //     return photo;
      //   })
      // );

      // Irá retornar um array de promises, ou seja, com erro
      // const postFormatted = currentPhotos.map(async (photo) => {
      //   const album = await axios.get(
      //     `https://jsonplaceholder.typicode.com/albums/${photo.albumId}`
      //   );

      //   photo.album = album.data;

      //   return photo;
      // });
    },

    allPostsSearch() {
      const searchTitle = this.searchTitle;
      const posts = this.posts;

      if (!searchTitle) {
        return posts;
      }

      return posts.filter((post) =>
        post.title.toUpperCase().includes(searchTitle.toUpperCase())
      );
    },
  },
  created() {
    this.getAllPostsAndPhotos();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 30px;
}

.post-card {
  background-color: #ccc;
}

.d-flex {
  display: flex;
}

.justify-content-center {
  justify-content: center;
}

.mb-5 {
  margin-bottom: 1rem;
}

.input-search {
  padding: 5px;
}

img {
  width: 100%;
}
</style>
