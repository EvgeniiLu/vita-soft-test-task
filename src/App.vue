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
          :visible.sync="visiblePostInfo"
          :fullscreen="true"
        >
          <post-info :openedPost="openedPost" @deleteComment="deleteComment" />
          <comment-form @addComment="addComment" />
        </el-dialog>
      </div>

      <div class="modal-post-form">
        <div class="create-post-btn" @click="createPost">
          <el-tooltip effect="light" content="Создать пост" placement="left">
            <div class="el-icon-circle-plus" />
          </el-tooltip>
        </div>
        <el-dialog
          :title="titlePostForm"
          :visible.sync="visiblePostForm"
          :fullscreen="true"
        >
          <post-form
            @addPost="addPost"
            :postObj="postObj"
            :btnName="btnNamePostForm"
          />
        </el-dialog>
      </div>
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

  mounted: function () {
    if (localStorage.length) {
      for (let i = 0; i < localStorage.length; i++) {
        this.posts.unshift(
          JSON.parse(localStorage.getItem(localStorage.key(i)))
        );
        this.posts.sort((a, b) => b.time - a.time);
      }
    }
  },

  data() {
    return {
      visiblePostInfo: false,

      visiblePostForm: false,

      titlePostForm: "",

      btnNamePostForm: "",

      openedPost: {},

      postObj: {},

      posts: [],
    };
  },

  methods: {
    createPost() {
      this.postObj = {
        title: "",
        desc: "",
        text: "",
        comments: [],
      };
      this.visiblePostForm = true;
      this.titlePostForm = "Создать пост";
      this.btnNamePostForm = "Добавить";
    },

    editPost(post) {
      this.postObj = post;
      this.visiblePostForm = true;
      this.titlePostForm = "Редактировать пост";
      this.btnNamePostForm = "Редактировать";
    },

    addPost(post) {
      if ("time" in post) {
        this.posts.forEach((value, key) => {
          if (value.time === post.time) {
            this.posts.splice(key, 1, post);
            this.storageSetItem(post);
          }
        });
      } else {
        post.time = Date.now();
        this.posts.unshift(post);
        this.storageSetItem(post);
      }
      this.visiblePostForm = false;
    },

    openPost(key) {
      this.visiblePostInfo = true;
      this.openedPost = this.posts[key];
    },

    deletePost(post) {
      this.posts = this.posts.filter((value) => value !== post);
      localStorage.removeItem(`${post.time}`);
    },

    addComment(comment) {
      comment.time = Date.now();
      this.openedPost.comments.push(comment);
      this.storageSetItem(this.openedPost);
    },

    deleteComment(comment) {
      this.openedPost.comments = this.openedPost.comments.filter(
        (value) => value !== comment
      );
      this.storageSetItem(this.openedPost);
    },

    storageSetItem(item) {
      localStorage.removeItem(item.time);
      localStorage.setItem(`${item.time}`, JSON.stringify(item));
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

.create-post-btn {
  font-size: 60px;
  position: fixed;
  bottom: 50px;
  right: 50px;
  color: #409eff;
}

.create-post-btn:hover {
  opacity: 0.75;
}
</style>
