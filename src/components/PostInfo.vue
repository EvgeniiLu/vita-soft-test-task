<template>
  <div class="post-info">
    <div class="post-title">{{ openedPost.title }}</div>
    <div class="post-description">{{ openedPost.text }}</div>
    <div class="title-comments">Комментари:</div>
    <template v-if="openedPost.comments.length">
      <div class="comments-list">
        <div
          class="post-comment"
          v-for="item in openedPost.comments"
          :key="item.time"
        >
          <div class="comment-content">
            <div class="comment-name">{{ item.name }}</div>
            <div class="comment-text">{{ item.text }}</div>
          </div>
          <div class="delete-btn">
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
    <template v-else>
      <h5 class="empty">Нет комментариев</h5>
    </template>
  </div>
</template>

<script>
export default {
  name: "PostInfo",

  props: {
    openedPost: Object,
  },

  methods: {
    deleteComment(comment) {
      this.$emit("deleteComment", comment);
    },
  },
};
</script>
<style lang="scss" scoped>
.post-title {
  text-indent: 20px;
  font-size: 18px;
  font-weight: 600;
  padding: 10px 0;
}

.post-description {
  text-indent: 20px;
}

.title-comments {
  padding: 10px 0;
  font-style: italic;
}

.comments-list {
  border: 1px solid #dcdfe6;
  height: 30vh;
  overflow: auto;
  margin-bottom: 10px;
  border-radius: 5px;
}

.post-comment {
  padding: 10px 0;
  display: flex;
  flex-direction: row;

  .comment-content {
    width: 90%;
    padding: 5px;

    .comment-name {
      font-weight: 600;
      text-indent: 20px;
    }

    .comment-text {
      padding-top: 5px;
      text-indent: 20px;
    }
  }

  .delete-btn {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 10%;
  }
}

.empty {
  font-size: 25px;
  height: 30vh;
}
</style>
