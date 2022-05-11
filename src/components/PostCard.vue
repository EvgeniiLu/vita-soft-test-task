<template>
  <el-card shadow="hover" class="post-card">
    <div class="post-card-inner">
      <div class="post-card-left">
        <div class="post-title">{{ post.title }}</div>
        <div class="post-description">{{ post.desc }}</div>
        <div class="post-comments-counter">
          <el-badge :value="post.comments.length" type="primary">
            <div class="el-icon-chat-line-round" />
          </el-badge>
        </div>
      </div>
      <div class="post-card-right">
        <div class="post-creation">{{ timeOfCreation }}</div>
        <div class="post-btns">
          <el-button
            class="edit-btn"
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
              class="delete-btn"
              slot="reference"
              type="danger"
              icon="el-icon-delete"
              circle
              @click.stop
            ></el-button>
          </el-popconfirm>
        </div>
      </div>
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
<style lang="scss" scoped>
.post-card-inner {
  display: flex;
  justify-content: space-between;

  .post-card-left {
    width: 70%;

    .post-title {
      font-size: 18px;
      font-weight: 600;
    }

    .post-description {
      display: block;
      width: 100%;
      font-size: 15px;
      padding-top: 5px;
    }

    .post-comments-counter {
      padding-top: 15px;
    }
  }

  .post-card-right {
    width: 30%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: flex-end;

    .post-creation {
      margin-left: 10px;
    }
    .edit-btn {
      margin-left: 15px;
    }
    .delete-btn {
      margin-left: 15px;
    }
  }
}

.post-comments-counter {
  font-size: 32px;
}
</style>
