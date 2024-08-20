<template>
    <div>
      <h2>{{ isEditing ? 'Edit Product' : 'Add Product' }}</h2>
      <form @submit.prevent="submitForm">
        <input v-model="product.title" placeholder="Product title" />
        <button type="submit">{{ isEditing ? 'Update' : 'Add' }}</button>
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    props: ['productToEdit'],
    data() {
      return {
        product: {
          id: null,
          title: ''
        },
        isEditing: false
      };
    },
    watch: {
      productToEdit(newVal) {
        if (newVal) {
          this.product = { ...newVal };
          this.isEditing = true;
        } else {
          this.resetForm();
        }
      }
    },
    methods: {
      submitForm() {
        if (this.isEditing) {
          axios.put(`https://api.escuelajs.co/api/v1/products/${this.product.id}`, this.product)
            .then(() => {
              this.$emit('product-updated', this.product);
              this.resetForm();
            });
        } else {
          axios.post('https://api.escuelajs.co/api/v1/products', this.product)
            .then(response => {
              this.$emit('product-added', response.data);
              this.resetForm();
            });
        }
      },
      resetForm() {
        this.product = { id: null, title: '' };
        this.isEditing = false;
      }
    }
  };
  </script>
  