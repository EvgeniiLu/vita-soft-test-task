<template>
  <el-card shadow="hover">
    <div class="content">
      <div class="post-title">{{ post.title }}</div>
      <div class="post-description">{{ post.desc }}</div>
    </div>
    <div class="post-comments-counter">
      <el-badge :value="50" type="primary">
        <div class="el-icon-chat-line-round" />
      </el-badge>
    </div>
    <div class="edit">
      <el-button type="primary" icon="el-icon-edit" circle></el-button>

      <el-popconfirm
        confirm-button-text="OK"
        cancel-button-text="No, Thanks"
        icon="el-icon-info"
        title="Are you sure to delete this?"
      >
        <el-button
          slot="reference"
          type="danger"
          icon="el-icon-delete"
          circle
        ></el-button>
      </el-popconfirm>
    </div>
  </el-card>
</template>
<script>
export default {
  name: "PostCard",

  props: {
    post: Object,
  },
};
</script>
<style lang="scss" scoped></style>
