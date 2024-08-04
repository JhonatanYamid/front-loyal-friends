<template>
    <div>
      <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">
    <Navbar></Navbar>
      <Nuxt />
    </div>
  </template>
  

  
  <script>
import Header from '~/components/Navbar.vue';

export default {
  components: {
    Header,
  },
};
</script>
