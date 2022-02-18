<template>
  <div class="hello">
    <button class="btn btn-secondary" @click.prevent="addCategory()">
      Ajouter une nouvelle categorie
    </button>
    <br /><br />
    <hr />
    <br />
    <table class="table">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Categorie</th>
          <th scope="col">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="category in categories" :key="category.id">
          <th scope="row">
            <router-link :to="'/detailcategory/' + category.id">
              {{ category.id }}
            </router-link>
          </th>
          <td>{{ category.category }}</td>
          <td>
            <button class="btn btn-primary">Modifier</button>
            <button
              class="btn btn-danger"
              @click.prevent="deleteCategory(category.id)"
            >
              SUpprimer
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import Swal from "sweetalert2";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      categories: null,
    };
  },
  methods: {
    getAllCategories() {
      this.$axios.get("/category").then((res) => {
        console.log(res.data.data);
        this.categories = res.data.donnee;
      });
    },
    addCategory() {
      Swal.fire({
        title: "Add Category",
        html: `
        <input class="form-control form-control-lg" type="text" id="name" placeholder="Ajouter une categorie">

        `,
        showCancelButton: true,
        confirmButtonText: "Ajouter",
      }).then((result) => {
        if (result.isConfirmed) {
          let category = document.getElementById("name").value;
          var data = {
            category: category,
          };
          var config = {
            headers: { "Access-Control-Allow-Origin": "*" },
          };
          this.$axios
            .post("/addcategory", data, {
              headers: {
                "Access-Control-Allow-Origin": "*",
              },
            })
            .then(() => {
              this.getAllCategories();
            })
            .catch(function (error) {
              console.log(error);
              return Promise.reject(error);
            });
        }
      });
    },
    deleteCategory(id) {
      this.$axios.get("/deletecategory/" + id).then((res) => {
        this.getAllCategories();
      });
    },
  },
  mounted() {
    this.getAllCategories();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
