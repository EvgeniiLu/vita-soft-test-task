<template>
  <div class="post-info">
    <div class="post-title">{{ selected.title }}</div>
    <div class="post-description">{{ selected.text }}</div>
    <div
      class="post-comments"
      v-for="(item, key) in selected.comments"
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
    selected: Object,
  },
};
</script>
<style lang="scss" scoped></style>
