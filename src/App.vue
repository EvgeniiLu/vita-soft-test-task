<template>
  <div class="app">
    <div class="container">
      <template v-if="posts.length">
        <div
          class="post"
          @click="openPost(key)"
          v-for="(item, key) in posts"
          :key="item.time"
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
          <comment-form @addComment="addComment" />
        </el-dialog>
      </div>
      <post-form @addPost="addPost" :edit="edit" />
    </div>
  </div>
</template>

<script>
import PostCard from "./components/PostCard.vue";
import PostInfo from "./components/PostInfo.vue";
import CommentForm from "./components/CommentForm.vue";
import PostForm from "./components/PostForm.vue";

export default {
  name: "App",

  components: {
    PostCard,
    PostInfo,
    CommentForm,
    PostForm,
  },

  created: function () {
    if (localStorage.length) {
      for (let i = 0; i < localStorage.length; i++) {
        let key = localStorage.key(i);
        this.posts.unshift(JSON.parse(localStorage.getItem(key)));
      }
    }
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
      if ("time" in post) {
        this.posts.forEach((value, key) => {
          if (value.time === post.time) {
            this.posts.splice(key, 1, post);
            localStorage[`${value.time}`] = JSON.stringify(post);
          }
        });
      } else {
        post.time = Date.now();
        this.posts.unshift(post);
        localStorage.setItem(`${post.time}`, JSON.stringify(post));
      }
    },

    deletePost(post) {
      this.posts = this.posts.filter((value) => value !== post);
      localStorage.removeItem(`${post.time}`);
    },

    editPost(post) {
      let { title, desc, text, comments, time } = post;
      this.edit = { title, desc, text, comments, time };
    },

    addComment(comment) {
      comment.time = Date.now();
      localStorage.removeItem(this.selectedPost.time);
      this.selectedPost.comments.push(comment);
      localStorage.setItem(
        `${this.selectedPost.time}`,
        JSON.stringify(this.selectedPost)
      );
    },

    deleteComment(comment) {
      this.selectedPost.comments = this.selectedPost.comments.filter(
        (value) => value !== comment
      );
      localStorage.setItem(
        `${this.selectedPost.time}`,
        JSON.stringify(this.selectedPost)
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
