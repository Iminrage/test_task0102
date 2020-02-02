<template>
  <div class="post--comment post">
    <div class="post__content">
      <img class="post__img" src="http://placekitten.com/100/100" width="100" height="100" />
      <div class="post__text">
        <span class="post__author post__author--comment">
          {{ comment.name }}
          <span class="post__answer-mark">промурчал</span>
        </span>
        <p class="post__body">{{ comment.body }}</p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Comment",
  props: {
    comment: {
      type: Object,
      required: true
    }
  }
};
</script>