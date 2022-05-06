<template>
  <div class="app">
    <div class="container">
      <template v-if="posts.length">
        <div
          class="post"
          @click="dialogVisible = true"
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

      <div class="modal-post">
        <el-dialog title="Tips" :visible.sync="dialogVisible" width="90%">
          <div class="post-title">lorem</div>
          <div class="post-description">lorem</div>
          <div class="post-comments">lorem</div>

          <div class="form">
            <el-form
              :model="ruleForm"
              :rules="rules"
              ref="ruleForm"
              label-width="120px"
              class="demo-ruleForm"
            >
              <el-form-item label="Activity name" prop="name">
                <el-input v-model="ruleForm.name"></el-input>
              </el-form-item>
              <el-form-item label="Activity form" prop="desc">
                <el-input type="textarea" v-model="ruleForm.desc"></el-input>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')"
                  >Create</el-button
                >
              </el-form-item>
            </el-form>
          </div>
        </el-dialog>
      </div>
      <modal-post-area @add="addPost" />
    </div>
  </div>
</template>

<script>
import ModalPostArea from "./components/ModalPostArea.vue";

export default {
  name: "App",
  components: {
    ModalPostArea,
  },

  data() {
    return {
      posts: [
        {
          title: "1",
          desc: "2",
          text: "3",
        },
        {
          title: "1",
          desc: "2",
          text: "3",
        },
      ],

      dialogVisible: false,

      ruleForm: {
        name: "",
        desc: "",
      },
      rules: {
        name: [
          {
            required: true,
            message: "Please input Activity name",
            trigger: "blur",
          },
          {
            min: 3,
            max: 5,
            message: "Length should be 3 to 5",
            trigger: "blur",
          },
        ],
        desc: [
          {
            required: true,
            message: "Please input activity form",
            trigger: "blur",
          },
        ],
      },
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },

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
