<template>
  <div class="post-info">
    <div class="post-title">{{ selectedPost.title }}</div>
    <div class="post-description">{{ selectedPost.text }}</div>
    <div
      class="post-comments"
      v-for="(item, key) in selectedPost.comments"
      :key="key"
    >
      <div class="comment-name">{{ item.name }}</div>
      <div class="comment-text">{{ item.text }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "PostInfo",

  props: {
    selectedPost: Object,
  },
};
</script>
<style lang="scss" scoped></style>
