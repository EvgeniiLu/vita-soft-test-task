<template>
  <div class="modal-post-info">
    <el-dialog title="Tips" :visible.sync="visiblePost" width="90%">
      <div class="post-title">post.title</div>
      <div class="post-description">post.desc</div>
      <div class="post-comments">post.text</div>

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
</template>
<script>
export default {
  name: "ModalPostInfo",

  props: {
    visible: Boolean,
  },

  watch: {
    visible() {
      this.visiblePost = this.visible;
    },
    visiblePost() {
      this.$emit("vis", this.visiblePost);
    },
  },

  data() {
    return {
      visiblePost: false,

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
  },
};
</script>

<style lang="scss" scoped></style>
