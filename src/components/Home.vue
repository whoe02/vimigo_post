<template>
  <el-container>
    <el-main>
      <img class="logo" src="../assets/vimigo.png" alt="Vimigo logo" />
      <div>
        <el-form :inline="true">
          <el-form-item label="User Id">
            <el-input
              type="text"
              v-model="s_user_id"
              placeholder="Enter Users Id"
            />
          </el-form-item>
          <el-form-item label="Title">
            <el-input type="text" v-model="s_title" placeholder="Enter any words of title" />
          </el-form-item>
          <el-form-item label="Body">
            <el-input type="text" v-model="s_body" placeholder="Enter any words inside body" />
          </el-form-item>
          <el-form-item>
            <el-button type="primary" plain @click="searchPost()"
              >Search</el-button
            >
            <el-button type="info" plain @click="loadData()">Reset</el-button>
          </el-form-item>
        </el-form>
          <AddPost @done="loadData" />
      </div>
      <div>
      </div>
      <table class="rwd-table">
        <thead>
          <tr>
            <th>Id</th>
            <th>User Id</th>
            <th>Title</th>
            <th>Body</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in posts" :key="item.id">
            <td data-th="Id">{{ item.id }}</td>
            <td data-th="User Id">{{ item.user_id }}</td>
            <td data-th="Title">{{ item.title }}</td>
            <td data-th="Body">{{ item.body }}</td>
            <td data-th="Action">
              <UpdatePost @done="loadData" :ItemId="item" />
              <DeletePost @done="loadData" :ItemId="item" />
            </td>
          </tr>
        </tbody>
      </table>
      <paginate
        v-model="page"
        :page-count="totalPages"
        :page-range="3"
        :margin-pages="2"
        :click-handler="getResults"
        :prev-text="'Prev'"
        :next-text="'Next'"
        :container-class="'pagination'"
        :page-class="'page-item'"
      >
      </paginate>
    </el-main>
  </el-container>
</template>

<script>
import axios from "axios";
import AddPost from "./AddPost.vue";
import UpdatePost from "./UpdatePost.vue";
import DeletePost from "./DeletePost.vue";
import Paginate from "vuejs-paginate-next";
export default {
  name: "Home",
  data() {
    return {
      posts: [],
      s_user_id: "",
      s_title: "",
      s_body: "",
      totalPages: 0,
      page: 0,
    };
  },
  components: {
    paginate: Paginate,
    AddPost,
    UpdatePost,
    DeletePost,
  },
  methods: {
    searchPost() {
      var params = "?page=" + this.page;
      if (this.s_user_id != "") {
        params = params + "&user_id=" + this.s_user_id;
      }

      if (this.s_title != "") {
        params = params + "&title=" + this.s_title;
      }

      if (this.s_body != "") {
        params = params + "&body=" + this.s_body;
      }
      this.loadData(params, false);
    },

    getResults(page = 1) {
      axios
        .get("https://gorest.co.in/public/v2/posts?page=" + page)
        .then((response) => {
          this.posts = response.data;
        })
    },
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
            this.loadData();
          }
        })
        .catch(function (error) {
        });

    },
    async loadData(params = "", reset = true) {
      const token =
        "efb5103ad5dc4a1e31dee2e5a1f8c81e183023a2c8cad4d696cf23a7d32a7131";
      let result = await axios.get(
        "https://gorest.co.in/public/v2/posts" + params,
        {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        }
      );
      this.posts = result.data;
      this.totalPages = result.headers["x-pagination-pages"];
      if (reset) {
        this.s_user_id = "";
        this.s_title = "";
        this.s_body = "";
      }
    },
  },
  async mounted() {
    this.loadData();
  },
};
</script>

<style>
@import "https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css";
.pagination{
  justify-content: center;
  margin-top: 2rem;
}
.logo {
  max-width: 100%;
  width: 500px;
  margin: auto;
}

.btn-container{
  margin-bottom: 1rem;
  text-align: left;
}

.rwd-table {
  margin: auto;
  min-width: 100%;
  max-width: 100%;
  border-collapse: collapse;
}

.rwd-table thead tr {
  border-top: none;
  background: #909399;
  color: #fff;
}

.rwd-table tr {
  border-top: 1px solid #ddd;
  border-bottom: 1px solid #ddd;
  background-color: #f5f9fc;
}

.rwd-table tr:nth-child(odd):not(:first-child) {
  background-color: #ebf3f9;
}

.rwd-table th {
  display: none;
}

.rwd-table td {
  display: block;
}
.rwd-table td .el-button{
  margin: .2rem;
}

.rwd-table td:first-child {
  margin-top: 0.5em;
}

.rwd-table td:last-child {
  margin-bottom: 0.5em;
}

.rwd-table td:before {
  content: attr(data-th) ": ";
  font-weight: bold;
  width: 120px;
  display: inline-block;
  color: #000;
}

.rwd-table th,
.rwd-table td {
  text-align: left;
}

.rwd-table {
  color: #333;
  border-radius: 0.4em;
  overflow: hidden;
}

.rwd-table tr {
  border-color: #bfbfbf;
}

.rwd-table th,
.rwd-table td {
  padding: 0.5em 1em;
}
@media screen and (max-width: 601px) {
  .rwd-table tr:nth-child(2) {
    border-top: none;
  }
}
@media screen and (min-width: 600px) {
  .rwd-table tr:hover:not(:first-child) {
    background-color: #d8e7f3;
  }
  .rwd-table td:before {
    display: none;
  }
  .rwd-table th,
  .rwd-table td {
    display: table-cell;
    padding: 0.25em 0.5em;
  }
  .rwd-table th:first-child,
  .rwd-table td:first-child {
    padding-left: 0;
  }
  .rwd-table th:last-child,
  .rwd-table td:last-child {
    padding-right: 0;
  }
  .rwd-table th,
  .rwd-table td {
    padding: 1em !important;
  }
}
</style>
