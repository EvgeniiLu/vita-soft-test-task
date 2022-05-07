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
          <post-card :post="item" @postdelete="deletePost" />
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
          <post-info :selected="selectedPost" />
          <add-comments-form @commentAdd="addCommentFunc" />
        </el-dialog>
      </div>
      <add-post-form @postAdd="addPost" />
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
            {
              name: "4",
              text: "5",
            },
            {
              name: "4",
              text: "5",
            },
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
      console.log(this.selectedPost);
    },

    addPost(post) {
      this.posts.unshift(post);
    },

    addCommentFunc(comment) {
      this.selectedPost.comments.push(comment);
    },

    deletePost(post) {
      this.posts = this.posts.filter((value) => value !== post);
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
