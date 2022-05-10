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
          <post-card
            :post="item"
            @deletePost="deletePost"
            @editPost="editPost"
          />
        </div>
      </template>
      <template v-else>
        <h1 class="empty">Нет постов</h1>
      </template>

      <div class="modal-post-info">
        <el-dialog
          title="Информация о посте"
          :visible.sync="visiblePost"
          width="90%"
        >
          <post-info
            :selectedPost="selectedPost"
            @deleteComment="deleteComment"
          />
          <add-comments-form @addComment="addComment" />
        </el-dialog>
      </div>
      <add-post-form @addPost="addPost" :edit="edit" />
    </div>
  </div>
</template>

<script>
import PostCard from "./components/PostCard.vue";
import PostInfo from "./components/PostInfo.vue";
import AddCommentsForm from "./components/AddCommentsForm.vue";
import AddPostForm from "./components/AddPostForm.vue";

export default {
  name: "App",

  components: {
    PostCard,
    PostInfo,
    AddCommentsForm,
    AddPostForm,
  },

  data() {
    return {
      visiblePost: false,

      selectedPost: {},

      edit: {},

      posts: [],
    };
  },

  methods: {
    openPost(key) {
      this.visiblePost = true;
      this.selectedPost = this.posts[key];
    },

    addPost(post) {
      if ("index" in post) {
        this.posts.splice(post.index, 1, post);
      } else this.posts.unshift(post);
    },

    deletePost(post) {
      this.posts = this.posts.filter((value) => value !== post);
    },

    editPost(post) {
      let { title, desc, text, comments } = post;
      this.edit = { title, desc, text, comments };
      this.edit.index = this.posts.indexOf(post);
    },

    addComment(comment) {
      this.selectedPost.comments.push(comment);
    },

    deleteComment(comment) {
      this.selectedPost.comments = this.selectedPost.comments.filter(
        (value) => value !== comment
      );
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
