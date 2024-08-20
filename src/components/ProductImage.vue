<template>
    <div>
      <h2>{{ product.title }}</h2>
      <img :src="product.images[0]" alt="Product Image" v-if="product.images.length > 0" />
      <p>{{ product.description }}</p>
      <p>Price: ${{ product.price }}</p>
      <p>Category: {{ product.category.name }}</p>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        product: {
          id: null,
          title: '',
          price: null,
          description: '',
          images: [],
          category: {
            id: null,
            name: '',
            image: ''
          }
        }
      };
    },
    methods: {
      fetchProduct() {
        axios.get('https://api.escuelajs.co/api/v1/products/16')
          .then(response => {
            this.product = response.data;
          })
          .catch(error => {
            console.error('Error fetching product:', error);
          });
      }
    },
    mounted() {
      this.fetchProduct();
    }
  };
  </script>
  
  <style scoped>
  /* Add any required styles here */
  </style>
  