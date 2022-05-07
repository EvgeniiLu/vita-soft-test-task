<template>
  <div class="form">
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm">
      <el-form-item prop="name">
        <el-input v-model="ruleForm.name" placeholder="Введите имя"></el-input>
      </el-form-item>
      <el-form-item prop="text">
        <el-input
          type="textarea"
          v-model="ruleForm.text"
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
  name: "AddCommentsForm",

  data() {
    return {
      ruleForm: {
        name: "",
        text: "",
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
            max: 20,
            message: "Length should be 3 to 5",
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
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.$emit("commentAdd", this.ruleForm);
          this.ruleForm = {
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
