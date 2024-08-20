<template>
    <div>
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
   img {
    width: 100%;
}

.categoryList{
    width: 100%;
    grid-auto-flow: column;
    overflow-x: auto;
    grid-auto-columns:  calc(calc(100% - 84px) / 5);
    display: grid;
    gap: 0 16px;
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
  