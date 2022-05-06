<template>
  <div class="app">
    <div class="container">
      <template v-if="posts.length">
        <div
          class="post"
          @click="openPost"
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

      <modal-post-area @add="addPost" />
      <modal-post-info :visible="postInfoVisible" @vis="toglePost" />
    </div>
  </div>
</template>

<script>
import ModalPostArea from "./components/ModalPostArea.vue";
import ModalPostInfo from "./components/ModalPostInfo.vue";

export default {
  name: "App",

  components: {
    ModalPostArea,
    ModalPostInfo,
  },

  data() {
    return {
      postInfoVisible: false,

      posts: [
        {
          title: "1",
          desc: "2",
          text: "3",
        },
      ],
    };
  },

  methods: {
    addPost(post) {
      this.posts.unshift(post);
    },

    openPost() {
      this.postInfoVisible = true;
    },

    toglePost(param) {
      this.postInfoVisible = param;
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
