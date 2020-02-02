<template>
  <div class="post">
    <div class="post__content">
      <img class="post__img" src="http://placekitten.com/g/200/200" width="200" height="200" />
      <div class="post__text">
        <span class="post__author">{{ user.name }}</span>
        <h3 class="post__title">{{ post.title }}</h3>
        <p class="post__body">{{ post.body }}</p>
      </div>
    </div>
    <div class="comments-section" v-if="commentsIsVisible">
      <Comment v-for="comment in comments" :comment="comment" :key="comment.id" />
    </div>
    <button
      :class="commentsIsVisible ? 'post__comments-btn post__comments-btn--close' : 'post__comments-btn'"
      @click="oCComments"
    >{{ commentsIsVisible ? "Закрыть" : "Открыть"}} комментарии</button>
  </div>
</template>
<script>
import axios from "axios";
import Comment from "./Comment.vue";
export default {
  name: "Post",
  components: {
    Comment
  },
  props: {
    post: {
      type: Object,
      required: true
    },
    user: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      commentsIsVisible: false,
      loading: false,
      comments: []
    };
  },
  methods: {
    getComments() {
      this.loading = true;
      axios
        .get(
          `https://jsonplaceholder.typicode.com/comments?postId=${this.post.id}`
        )
        .then(response => {
          this.comments = response.data;
        })
        .catch(err => {
          console.log(err);
        })
        .finally(() => {
          this.loading = false;
          this.commentsIsVisible = !this.commentsIsVisible;
        });
    },
    oCComments() {
      if (this.comments.length) {
        this.commentsIsVisible = !this.commentsIsVisible;
      } else {
        this.getComments();
      }
    }
  }
};
</script>