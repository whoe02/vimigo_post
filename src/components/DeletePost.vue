<template>
  <el-button type="danger" circle @click="dialogVisible = true"
    ><i class="fas fa-trash"></i
  ></el-button>

  <el-dialog
    v-model="dialogVisible"
    title="Delete Post"
    width="50%"
  >
    <div>
      <p>Are you sure to delete the post id = {{ ItemId.id }}</p>
    </div>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogVisible = false">Cancel</el-button>
        <el-button type="primary" @click="deletePost(ItemId)"
          >Confirm</el-button
        >
      </span>
    </template>
  </el-dialog>
</template>

<script>
import axios from "axios";
export default {
  name: "DeletePost",
  props: ["ItemId"],
  data() {
    return {
      dialogVisible: false,
      user_id: this.ItemId.user_id,
      title: this.ItemId.title,
      body: this.ItemId.body,
    };
  },

  methods: {
    async deletePost(item) {
      const token =
        "efb5103ad5dc4a1e31dee2e5a1f8c81e183023a2c8cad4d696cf23a7d32a7131";
      let result = await axios
        .delete("https://gorest.co.in/public/v2/posts/" + item.id, {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        })
        .then((response) => {
          if (response.status == 204) {
            this.dialogVisible = false;
            this.$emit("done");
          }
        })
        .catch(function (error) {
        });

    },
  },
};
</script>
