<template>
  <div class="app">
    <div class="container">
      <template v-if="posts.length">
        <div class="posts-block">
          <div class="post-list">
            <div
              class="post"
              @click="openPost(key)"
              v-for="(item, key) in postsList"
              :key="item.time"
            >
              <post-card
                :post="item"
                @deletePost="deletePost"
                @editPost="editPost"
              />
            </div>
          </div>

          <div class="pag">
            <el-pagination
              @current-change="setPage"
              :current-page="page"
              background
              layout="prev, pager, next"
              :total="pageSize"
            />
          </div>
        </div>
      </template>
      <template v-else>
        <div class="empty"><h1>Нет постов</h1></div>
      </template>

      <div class="modal-post-info">
        <el-dialog
          title="Информация о посте"
          :visible.sync="visiblePostInfo"
          width="60%"
          top="10vh"
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
          width="60%"
          top="15vh"
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

      page: 1,
    };
  },

  computed: {
    pageSize() {
      return Math.ceil((this.posts.length / 6) * 10);
    },

    start() {
      return (this.page - 1) * 6;
    },

    end() {
      return this.page * 6 - 1;
    },

    postsList() {
      return this.posts.slice(this.start, this.end);
    },
  },

  watch: {
    pageSize() {
      if (Number.isInteger(this.pageSize / 10)) this.page = this.pageSize / 10;
    },
  },

  methods: {
    setPage(val) {
      this.page = val;
    },

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
      let { title, desc, text, comments, time } = post;
      this.postObj = { title, desc, text, comments, time };
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
      this.page = 1;
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

<style lang="scss">
body {
  margin: 0;
  font-size: 15px;
  color: #6c7279;
  background-color: #fff;
}

*,
*:before,
*:after {
  box-sizing: border-box;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  margin: 0;
}

p {
  margin: 0;
}

.container {
  width: 100%;
  max-width: 1300px;
  margin: 0 auto;
}

.posts-block {
  width: 100%;
  height: 100vh;
  position: relative;
  padding: 10px;

  .post-list {
    display: flex;
    flex-direction: column;
    min-height: 85%;

    .post {
      padding-bottom: 10px;

      :hover {
        cursor: pointer;
      }
    }
  }

  .pag {
    min-height: 15%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
}

.create-post-btn {
  font-size: 60px;
  position: fixed;
  bottom: 50px;
  right: 50px;
  color: #409eff;

  :hover {
    transform: scale(1.1);
    opacity: 0.75;
    transition: 0.2s linear;
  }
}

.empty {
  font-size: 50px;
  color: #303133;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.el-badge__content.is-fixed {
  border: none;
}
</style>
