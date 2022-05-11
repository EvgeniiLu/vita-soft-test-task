<template>
  <div class="form">
    <el-form :model="form" :rules="rules" ref="ruleForm">
      <el-form-item prop="name">
        <el-input
          v-model="form.name"
          placeholder="Введите имя"
          maxlength="30"
          show-word-limit
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
          >Отправить</el-button
        >
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  name: "AddCommentForm",

  data() {
    return {
      form: {
        name: "",
        text: "",
      },

      rules: {
        name: [
          {
            required: true,
            message: "Введите имя",
            trigger: "blur",
          },
          {
            min: 3,
            message: "Имя должно содержать не менее 3 символов",
            trigger: "blur",
          },
        ],

        text: [
          {
            required: true,
            message: "Введите комментарий",
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
          this.$emit("addComment", this.form);

          this.form = {
            name: "",
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

<style lang="scss" scoped></style>
