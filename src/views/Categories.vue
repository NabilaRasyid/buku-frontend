<template>
  <div>
    <h1>Categories</h1>
    <!-- Tampilkan daftar kategori -->
    <table class="table">
      <thead>
      <tr class="table-secondary">
        <th>Code</th>
        <th>Categories</th>
        <th>Action</th>
      </tr>
      </thead>
      <tbody class="table-light">
      <tr v-for="category in categories" :key="category.id">
        <td>{{ category.kode }}</td>
        <td>{{ category.kategori }}</td>
        <td>
          <router-link :to="`/categories/${category.kode}`" class="btn btn-info">Edit</router-link>
          <button @click="deleteCategory(category.kode)" class="btn btn-danger">Delete</button>
        </td>
      </tr>
      </tbody>
    </table>
    <router-link to="/categories/add" class="btn btn-success">Add Category</router-link>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      categories: [],
    };
  },
  methods: {
    fetchCategories() {
      axios.get('http://localhost/buku-backend/selectKategori.php')
          .then(response => {
            this.categories = response.data;
          })
          .catch(error => {
            console.error(error);
          });
    },
    deleteCategory(kode) {
      if (confirm('Are you sure you want to delete this category?')) {
        axios.delete(`http://localhost/buku-backend/deleteKategoribyKode.php?kode=${kode}`)
            .then(() => {
              alert('Category deleted successfully');
              this.fetchCategories();
            })
            .catch(error => {
              console.error(error);
            });
      }
    },

  },
  created() {
    this.fetchCategories();
  },
};
</script>
