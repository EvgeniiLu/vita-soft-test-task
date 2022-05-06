<template>
  <div class="modal-add-post">
    <div class="add-post" @click="createPost">
      <el-tooltip effect="light" content="Add Post" placement="left">
        <div class="el-icon-circle-plus" />
      </el-tooltip>
    </div>
    <el-dialog :title="titlePost" :visible.sync="dialogVisible" width="90%">
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
            <el-button type="primary" @click="submitForm('ruleForm')"
              >Добавить</el-button
            >
          </el-form-item>
        </el-form>
      </div>
    </el-dialog>
  </div>
</template>
<script>
export default {
  name: "ModalPostArea",

  data() {
    return {
      titlePost: "",

      dialogVisible: false,
      form: {
        title: "",
        desc: "",
        text: "",
      },

      rules: {
        title: [
          {
            required: true,
            message: "Please input Activity name",
            trigger: "blur",
          },
          {
            min: 3,
            max: 20,
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
        text: [
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
    createPost() {
      this.dialogVisible = true;
      this.titlePost = "Создать пост";
    },

    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.$emit("add", this.form);
          this.dialogVisible = false;
          this.form = {
            title: "",
            desc: "",
            text: "",
          };
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
