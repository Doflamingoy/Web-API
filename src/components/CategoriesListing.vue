<template>
    <div class = "category-wrapper">
      <div class = "category-header">
        <h1>Categories</h1>
      </div>
      <div v-if="loading">Loading...</div>
      <div v-if="error">{{ error }}</div>
      <ul class="fetched-api" v-if="data && !loading">
        <div class = "categoryList">
        <li v-for="categories in data" :key="categories.id" class="git-user">
            <img :src="categories.image" alt="Category Image" v-if="categories.image.length > 0" />
            {{ categories.name }} 
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
        error: null
      }
    },
    methods: {
      fetchData() {
        fetch('https://api.escuelajs.co/api/v1/categories')
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
      }
    },
    created() {
      this.fetchData();
    }
  }
  </script>
  
  <style scoped>

  .category-wrapper {
    max-width: 1200px;
    margin: auto;
  }

  .category-wrapper .category-header h1 {
    text-transform: uppercase;
    font-size: 16px;
  }

  .category-wrapper .categoryList li {
    display: flex;
    flex-direction: column;
  }

  .category-wrapper .categoryList img{
    height: 45px;
    width: 45px;
  }

.categoryList{
    grid-auto-flow: column;
    display: grid;
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
  /* Add your styles here */
  </style>
  