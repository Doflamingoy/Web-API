<template>
    <div>
    <input v-model="searchTerm" @input="filterItems" placeholder="Search products" />
        <div class="productListing">
            <ul>
                <div class = "menuList">
                    <li v-for="product in filteredProducts" :key="product.id">
                        <div class = "productInfo">
                            <img :src="product.images[0]" alt="Product Image" />
                            <p>${{ product.price }} </p>
                            <p>{{ product.title }} </p>
                            <button @click="editProduct(product)">Edit</button>
                            <button class ="deleteBtn" @click="deleteProduct(product.id)">Delete</button>
                        </div>
                    </li>
                </div>
            </ul>
        </div>
    </div>

  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        products: [],
        searchTerm: '',
        filteredProducts: []
      };
    },
    methods: {
      fetchProducts() {
        axios.get('https://api.escuelajs.co/api/v1/products')
          .then(response => {
            this.products = response.data;
            this.filteredProducts = this.products;
          });
      },
      filterItems() {
        this.filteredProducts = this.products.filter(product =>
          product.title.toLowerCase().includes(this.searchTerm.toLowerCase())
        );
      },
      editProduct(product) {
        this.$emit('edit-product', product);
      },
      deleteProduct(id) {
        axios.delete(`https://api.escuelajs.co/api/v1/products/${id}`)
          .then(() => {
            this.products = this.products.filter(product => product.id !== id);
            this.filterItems();
          });
      }
    },
    mounted() {
      this.fetchProducts();
    }
  };
  </script>
  
<style scoped>
img {
    width: 100%;
}

.menuList{
    grid-template-columns: repeat(6, 1fr);
    grid-auto-flow: row;
    grid-auto-columns: auto;
    gap: 20px 10px;
    display: grid;
}

ul{
    list-style-type: none;
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

.productListing{
    max-width: 1400px;
    margin: auto;
}

</style>