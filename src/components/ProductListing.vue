<template>
    <div>
    <div>
        <input v-model="searchTerm" @input="filterItems" placeholder="Search products" />
    </div>
      <div v-if="loading">Loading...</div>
      <div v-if="error">{{ error }}</div>
      <ul class="fetched-api" v-if="filteredProducts && !loading">
        <div class="productList">
          <li v-for="product in filteredProducts" :key="product.id" class="git-user">
            <img :src="product.images[0]" alt="Product Image" v-if="product.images.length > 0" />
            <p>{{ product.title }}</p>
            <p>${{ product.price }}</p>
          </li>
        </div>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    name: 'APIFetching',
    data() {
      return {
        data: null,
        loading: true,
        searchTerm: '',
        error: null
      }
    },
    computed: {
      filteredProducts() {
        if (!this.data) return [];
        return this.data.filter(product =>
          product.title.toLowerCase().includes(this.searchTerm.toLowerCase())
        );
      }
    },
    methods: {
      fetchData() {
        fetch('https://api.escuelajs.co/api/v1/products')
          .then(response => {
            if (!response.ok) {
              throw new Error('Network response was not ok ' + response.statusText);
            }
            return response.json();
          })
          .then(data => {
            this.data = data;
            this.loading = false;
          })
          .catch(error => {
            this.error = error.toString();
            this.loading = false;
          });
      },
      filterItems() {
        // No longer needed as filtering is handled by the computed property
      }
    },
    created() {
      this.fetchData();
    }
  }
  </script>
  
  <style scoped>
  /* Your existing styles remain unchanged */
  img {
    width: 100%;
}

.productList{
    width: 100%;
    grid-template-columns: repeat(6, 1fr);
    grid-auto-flow: row;
    grid-auto-columns: auto;
    gap: 20px 10px;
    display: grid;
    gap: 0 12px;
}

ul{
    list-style-type: none;
    padding: 0;
    
}

p{
    font-size: 16px;
}

p:nth-child(3){
    font-weight: bold;
}

.deleteBtn{
    background-color: #f44336;
    border-radius: 8px;
    color: #fff;
    border: none;
    
}

button{
    width: 50%;
    height: 28px;
}
  </style>
  