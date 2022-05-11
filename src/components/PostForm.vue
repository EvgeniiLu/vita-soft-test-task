<template>
  <div class="form">
    <el-form :model="form" :rules="rules" ref="ruleForm">
      <el-form-item prop="title">
        <el-input
          v-model="form.title"
          placeholder="Заголовок"
          maxlength="30"
          show-word-limit
        ></el-input>
      </el-form-item>
      <el-form-item prop="desc">
        <el-input
          v-model="form.desc"
          placeholder="Краткое описание"
          maxlength="100"
          show-word-limit
        ></el-input>
      </el-form-item>
      <el-form-item prop="text">
        <el-input
          type="textarea"
          v-model="form.text"
          placeholder="Введите текст"
          :rows="7"
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">
          {{ btnName }}
        </el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  name: "AddPostForm",

  props: {
    postObj: Object,
    btnName: String,
  },

  created: function () {
    if (Object.keys(this.postObj).length) this.form = this.postObj;
  },

  watch: {
    postObj() {
      this.form = this.postObj;
    },
  },

  data() {
    return {
      form: {},

      rules: {
        title: [
          {
            required: true,
            message: "Введите название поста",
            trigger: "blur",
          },
          {
            min: 3,
            message: "Название поста должно содержать не менне 3 символов",
            trigger: "blur",
          },
        ],

        desc: [
          {
            required: true,
            message: "Введите описание",
            trigger: "blur",
          },
          {
            min: 5,
            message: "Описание поста должно содержать не менне 5 символов",
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
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.$emit("addPost", this.form);
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
