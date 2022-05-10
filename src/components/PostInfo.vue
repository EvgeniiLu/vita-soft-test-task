<template>
  <div class="post-info">
    <div class="post-title">{{ selectedPost.title }}</div>
    <div class="post-description">{{ selectedPost.text }}</div>
    <div class="post-comments">
      <div v-for="(item, key) in selectedPost.comments" :key="key">
        <div class="comment-name">{{ item.name }}</div>
        <div class="comment-text">{{ item.text }}</div>
        <el-popconfirm
          confirm-button-text="Да"
          cancel-button-text="Нет"
          icon="el-icon-info"
          title="Удалить комментарий?"
          @confirm="deleteComment(item)"
        >
          <el-button
            slot="reference"
            type="danger"
            icon="el-icon-delete"
            circle
          ></el-button>
        </el-popconfirm>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "PostInfo",

  props: {
    selectedPost: Object,
  },

  methods: {
    deleteComment(comment) {
      this.$emit("deleteComment", comment);
    },
  },
};
</script>
<style lang="scss" scoped></style>
