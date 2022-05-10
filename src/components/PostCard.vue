<template>
  <el-card shadow="hover">
    <div class="content">
      <div class="post-title">{{ post.title }}</div>
      <div class="post-description">{{ post.desc }}</div>
    </div>
    <div class="post-comments-counter">
      <el-badge :value="post.comments.length" type="primary">
        <div class="el-icon-chat-line-round" />
      </el-badge>
    </div>
    <div class="post-creation">{{ timeOfCreation }}</div>
    <div class="post-btns">
      <el-button
        type="primary"
        icon="el-icon-edit"
        circle
        @click.stop="editPost"
      ></el-button>

      <el-popconfirm
        confirm-button-text="Да"
        cancel-button-text="Нет"
        icon="el-icon-info"
        title="Удалить пост?"
        @confirm="deletePost"
      >
        <el-button
          slot="reference"
          type="danger"
          icon="el-icon-delete"
          circle
          @click.stop
        ></el-button>
      </el-popconfirm>
    </div>
  </el-card>
</template>
<script>
import dayjs from "dayjs";

export default {
  name: "PostCard",

  props: {
    post: Object,
  },

  computed: {
    timeOfCreation() {
      return dayjs(this.post.time).format("DD.MM.YYYY HH:mm");
    },
  },

  methods: {
    deletePost() {
      this.$emit("deletePost", this.post);
    },

    editPost() {
      this.$emit("editPost", this.post);
    },
  },
};
</script>
<style lang="scss" scoped></style>
