<template>
     <el-button type="primary" circle @click="dialogVisible = true"
              ><i class="fas fa-edit"></i
            ></el-button>
    <el-dialog
      v-model="dialogVisible"
      title="Update Post"
      width="50%"
    >
      <el-form label-width="60px"> 
        <el-form-item label="User Id">
        <el-input type="text" v-model="user_id" placeholder="Enter Users Id" />
        </el-form-item>
        <el-form-item label="Title">
        <el-input type="text" v-model="title" placeholder="Enter Title" />
        </el-form-item>
        <el-form-item label="Body">
        <el-input type="textarea" rows="5" v-model="body" placeholder="Enter body" />
        </el-form-item>
      </el-form>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogVisible = false">Cancel</el-button>
          <el-button type="primary" @click="ConfirmButton(ItemId.id)">Confirm</el-button>
        </span>
      </template>
    </el-dialog>
</template>

<script>
import axios from "axios";
export default {
  name: "UpdatePost",
  props: ['ItemId'],
  data() {
    return {
      dialogVisible: false,
      user_id: this.ItemId.user_id,
      title: this.ItemId.title,
      body: this.ItemId.body,
    };
  },

  methods: {
    async ConfirmButton(id) {
      const token =
        "efb5103ad5dc4a1e31dee2e5a1f8c81e183023a2c8cad4d696cf23a7d32a7131";
      let result = await axios.put(
        "https://gorest.co.in/public/v2/posts/"+id,
        {
          user_id: this.user_id,
          title: this.title,
          body: this.body,
        },
        {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        }
      );

      this.dialogVisible = false;
      this.$emit('done')
    },
  },
};
</script>
