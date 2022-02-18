<template>
  <div>
    <h2>Detail category</h2>
    Nom : {{ category.category }}
    <p v-if="category.products.length > 0">produits :</p>
  </div>
</template>

<script>
export default {
  data() {
    return { id: null, category: null };
  },
  methods: {
    getDetailCategory() {
      this.$axios.get("/detailcategory/" + this.id).then((res) => {
        console.log(res.data);
        this.category = res.data.donnee;
        console.log(this.category);
      });
    },
  },
  mounted() {
    console.log(this.$route.params.id);
    this.id = this.$route.params.id;
    this.getDetailCategory();
  },
};
</script>

<style></style>
