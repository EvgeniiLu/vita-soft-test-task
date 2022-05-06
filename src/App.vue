<template>
  <div class="app">
    <div class="container">
      <template v-if="posts.length">
        <div
          class="post"
          @click="openPost(key)"
          v-for="(item, key) of posts"
          :key="key"
        >
          <el-card shadow="hover">
            <div class="content">
              <div class="post-title">{{ item.title }}</div>
              <div class="post-description">{{ item.desc }}</div>
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
        </div>
      </template>

      <template v-else>
        <h1>Нет постов</h1>
      </template>

      <div class="modal-post-info">
        <el-dialog
          title="Информация о посте"
          :visible.sync="visiblePost"
          width="90%"
        >
          <div class="post-title">{{ selectedPost.title }}</div>
          <div class="post-description">{{ selectedPost.text }}</div>
          <div class="post-comments">{{ selectedPost.comments }}</div>
          <add-comments-form @addcomment="addCommentFunc" />
        </el-dialog>
      </div>
      <add-post-modal @add="addPost" />
    </div>
  </div>
</template>

<script>
import AddPostModal from "./components/AddPostModal.vue";
import AddCommentsForm from "./components/AddCommentsForm.vue";

export default {
  name: "App",

  components: {
    AddPostModal,
    AddCommentsForm,
  },

  data() {
    return {
      visiblePost: false,
      selectedPost: {},
      posts: [
        {
          title: "1",
          desc: "2",
          text: "3",
          comments: "4",
        },
      ],
    };
  },

  methods: {
    addPost(post) {
      this.posts.unshift(post);
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  width: 100%;
  max-width: 1300px;
  margin: 0 auto;
}

.post-comments-counter {
  font-size: 32px;
}

.el-badge__content.is-fixed {
  border: none;
}
</style>
