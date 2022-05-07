<template>
  <div class="app">
    <div class="container">
      <template v-if="posts.length">
        <div
          class="post"
          @click="openPost(key)"
          v-for="(item, key) in posts"
          :key="key"
        >
          <post-card :post="item" />
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
          <div
            class="post-comments"
            v-for="(item, key) in selectedPost.comments"
            :key="key"
          >
            <div class="comment-name">{{ item.name }}</div>
            <div class="comment-text">{{ item.text }}</div>
          </div>
          <add-comments-form
            @addcomment="addCommentFunc"
            :selected="selectedPost"
          />
        </el-dialog>
      </div>
      <add-post-modal @add="addPost" />
    </div>
  </div>
</template>

<script>
import AddPostModal from "./components/AddPostModal.vue";
import AddCommentsForm from "./components/AddCommentsForm.vue";
import PostCard from "./components/PostCard.vue";

export default {
  name: "App",

  components: {
    AddPostModal,
    AddCommentsForm,
    PostCard,
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
          comments: [
            {
              name: "4",
              text: "5",
            },
          ],
        },
      ],
    };
  },

  methods: {
    openPost(key) {
      this.visiblePost = true;
      this.selectedPost = this.posts[key];
    },

    addPost(post) {
      this.posts.unshift(post);
    },

    addCommentFunc(comment) {
      console.log(comment);
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
