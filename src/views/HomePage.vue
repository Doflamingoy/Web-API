<template>
    <div>
       <HeadLiner />
       <CategoryListing />
       <ProductListing />
    </div>
  </template>
  
  <script>

  import ProductListing from '../components/ProductListing.vue';
  import HeadLiner from '../components/HeaderPage.vue';
  import CategoryListing from '../components/CategoriesListing.vue';
  
  export default {
    components: {
      ProductListing,
      HeadLiner,
      CategoryListing
    }
  };
  </script>
  