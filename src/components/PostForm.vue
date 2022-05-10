<template>
  <div class="add-post-modal">
    <div class="add-post" @click="createPost">
      <el-tooltip effect="light" content="Add Post" placement="left">
        <div class="el-icon-circle-plus" />
      </el-tooltip>
    </div>
    <el-dialog
      :title="titlePost"
      :visible.sync="dialogVisible"
      :fullscreen="true"
    >
      <div class="form">
        <el-form :model="form" :rules="rules" ref="ruleForm">
          <el-form-item prop="title">
            <el-input v-model="form.title" placeholder="Заголовок"></el-input>
          </el-form-item>
          <el-form-item prop="desc">
            <el-input
              v-model="form.desc"
              placeholder="Краткое описание"
            ></el-input>
          </el-form-item>
          <el-form-item prop="text">
            <el-input
              type="textarea"
              v-model="form.text"
              placeholder="Введите текст"
            ></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="submitForm('ruleForm')">
              {{ btnName }}
            </el-button>
          </el-form-item>
        </el-form>
      </div>
    </el-dialog>
  </div>
</template>
<script>
export default {
  name: "AddPostForm",

  props: {
    edit: Object,
  },

  watch: {
    edit() {
      this.dialogVisible = true;
      this.titlePost = "Редактировать пост";
      this.btnName = "Редактировать";
      this.form = this.edit;
    },
  },

  data() {
    return {
      titlePost: "",

      btnName: "",

      dialogVisible: false,

      form: {
        title: "",
        desc: "",
        text: "",
        comments: [],
      },

      rules: {
        title: [
          {
            required: true,
            message: "Введите название поста",
            trigger: "blur",
          },
          {
            min: 3,
            max: 20,
            message:
              "Название поста должно содержать не менне 3 и не более 20 символов",
            trigger: "blur",
          },
        ],

        desc: [
          {
            required: true,
            message: "Введите описание",
            trigger: "blur",
          },
        ],

        text: [
          {
            required: true,
            message: "Введите текст",
            trigger: "blur",
          },
        ],
      },
    };
  },

  methods: {
    createPost() {
      this.dialogVisible = true;
      this.titlePost = "Создать пост";
      this.btnName = "Добавить";
      this.form = {
        title: "",
        desc: "",
        text: "",
        comments: [],
      };
    },

    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.$emit("addPost", this.form);
          this.dialogVisible = false;
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
  },
};
</script>
<style lang="scss" scoped>
.add-post {
  font-size: 60px;
  position: fixed;
  bottom: 50px;
  right: 50px;
  color: #409eff;
}
.add-post:hover {
  opacity: 0.75;
}
</style>
