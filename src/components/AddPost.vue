<template>
  <div class="btn-container">
    <el-button type="primary" @click="dialogVisible = true"
      ><i class="fas fa-plus" style="margin-right: 5px"></i>Create
      Post</el-button
    >
    <el-dialog
      v-model="dialogVisible"
      title="Create Post"
      width="50%"
    >
      <el-form label-width="60px">
        <el-form-item label="User Id">
          <el-input
            type="text"
            v-model="user_id"
            placeholder="Enter Users Id"
          />
        </el-form-item>
        <el-form-item label="Title">
          <el-input type="text" v-model="title" placeholder="Enter Title" />
        </el-form-item>
        <el-form-item label="Body">
          <el-input
            type="textarea"
            rows="5"
            v-model="body"
            placeholder="Enter body"
          />
        </el-form-item>
      </el-form>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogVisible = false">Cancel</el-button>
          <el-button type="primary" @click="ConfirmButton">Confirm</el-button>
        </span>
      </template>
    </el-dialog>
  </div>
</template>

<script>
import axios from "axios";
import { h } from "vue";
import { ElMessage } from "element-plus";
export default {
  name: "Home",
  data() {
    return {
      dialogVisible: false,
      id: "",
      user_id: "",
      title: "",
      body: "",
    };
  },

  methods: {
    async ConfirmButton() {
      const token =
        "efb5103ad5dc4a1e31dee2e5a1f8c81e183023a2c8cad4d696cf23a7d32a7131";
      let result = await axios
        .post(
          "https://gorest.co.in/public/v2/posts",
          {
            id: this.id,
            user_id: this.user_id,
            title: this.title,
            body: this.body,
          },
          {
            headers: {
              Authorization: `Bearer ${token}`,
            },
          }
        )
        .then((response) => {
          if (response.status == 201) {
            this.dialogVisible = false;
            this.$emit("done");
          }
        })
        .catch(function (error) {
          if (error.response.status == 422) {
            ElMessage.error(error.response.data[0].field + ' ' + error.response.data[0].message);
          }
        });
    },
  },
};
</script>
