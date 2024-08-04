<template>
  <div>
    <div class="m-5">
      <div class="my-10">
        <nuxt-link to="/create" class="bg-blue-500 text-white px-4 py-2 rounded">Crear Nueva Mascota</nuxt-link>
      </div>
      <div class="mb-4">
        <input v-model="searchId" @keyup.enter="searchPet" type="number" placeholder="Buscar mascota por ID" class="form-input mt-1 block w-full"/>
        <button @click="searchPet" class="bg-blue-500 text-white px-4 py-2 rounded mt-2">Buscar</button>
      </div>
      <List :pets="pets" @pet-deleted="fetchPets" />
    </div>
  </div>
</template>

<script>
import List from '../components/List.vue';

export default {
  layout: 'custom',
  name: 'IndexPage',
  components: {
    List
  },
  async asyncData({ $axios }) {
    // Fetch pets data here as initial data
    const { data } = await $axios.get('/pet/findByStatus');
    return { pets: data };
  },
  data() {
    return {
      pets: []
    };
  },
  methods: {
    async fetchPets() {
      try {
        const { data } = await this.$axios.get('/pet/findByStatus');
        this.pets = data;
      } catch (error) {
        console.error('Error fetching pets:', error);
        this.pets = [];
      }
    },
    async searchPet() {
      if (!this.searchId) return;

      try {
        const { data } = await this.$axios.get(`/pet/${this.searchId}`);
        this.pets = data.name?[data]:[];
        this.searchId = null; // Clear search input
      } catch (error) {
        console.error('Error fetching pet:', error);
        this.pets = [];
      }
    }
  },
  async mounted() {
    // Ensure data is loaded when the component mounts
    await this.fetchPets();
  }
};
</script>

<style scoped>
/* Estilos personalizados */
</style>
