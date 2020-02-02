<template>
  <div class="home">
    <div class="credit">
      Сделано
      <a href="https://hh.ru/resume/d555c3d9ff05bc77ff0039ed1f6c3842566272">Шевчуком Е.А.</a>
      специально для ProContext
    </div>
    <div class="container">
      <div class="home__wrapper">
        <Preloader class="home__peloader" v-if="loading" :width="46" :height="46" />
        <div class="posts">
          <div class="posts-wrapper" v-if="!loading">
            <Post
              v-for="post in filteredPosts"
              :post="post"
              :user="srchUser(post.userId)"
              :key="post.id"
            />
          </div>
          <div
            v-if="!filteredPosts.length || !posts.length"
            class="post post--placeholder"
          >Постов не найдено</div>
        </div>
        <aside class="filters">
          <div class="filters__fields">
            <h2>Фильтры</h2>
            <label>
              <span class="filters__filter-name">Название/имя</span>
              <input class="filters__input" type="text" v-model="titleQuery" />
            </label>
            <label>
              <span class="filters__filter-name">Контент</span>
              <input class="filters__input" type="text" v-model="contentQuery" />
            </label>
          </div>
        </aside>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Post from "@/components/Post.vue";
import Preloader from "@/components/Preloader.vue";
export default {
  name: "home",
  components: {
    Post,
    Preloader
  },
  data() {
    return {
      loading: true,
      posts: [],
      users: [],
      titleQuery: "",
      contentQuery: ""
    };
  },
  computed: {
    filteredPosts() {
      let newPosts = this.posts;
      if (!this.titleQuery && !this.contentQuery) return newPosts;
      if (this.titleQuery) {
        let query = this.titleQuery.trim().toLowerCase();
        newPosts = newPosts.filter(post => {
          let user = this.srchUser(post.userId);
          return (
            post.title
              .trim()
              .toLowerCase()
              .indexOf(query) !== -1 ||
            user.name
              .trim()
              .toLowerCase()
              .indexOf(query) !== -1
          );
        });
      }
      if (this.contentQuery) {
        let query = this.contentQuery.toLowerCase();
        newPosts = newPosts.filter(post => {
          let newPostBody = post.body.replace(/(\r\n|\n|\r)/gm, " ");
          return newPostBody.toLowerCase().indexOf(query) !== -1;
        });
      }
      return newPosts;
    }
  },
  created() {
    this.getPosts();
    this.getUsers();
  },
  methods: {
    getUsers() {
      this.loading = true;
      axios
        .get("https://jsonplaceholder.typicode.com/users")
        .then(response => {
          this.users = response.data;
        })
        .catch(err => {
          console.log(err);
        })
        .finally(() => (this.loading = false));
    },
    srchUser(userId) {
      return this.users.find(user => user.id == userId);
    },
    getPosts() {
      this.loading = true;
      axios
        .get("https://jsonplaceholder.typicode.com/posts")
        .then(response => {
          this.posts = response.data;
        })
        .catch(err => {
          console.log(err);
        });
      /* .finally(() => (this.loading = false)); */
    }
  }
};
</script>
