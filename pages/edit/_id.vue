<template>
  <div class="container mx-auto py-8">
    <h1 class="text-2xl font-bold mb-4">Actualizar Mascota</h1>
    <form @submit.prevent="updatePet">
      <div class="mb-4">
        <label class="block text-gray-700">Nombre:</label>
        <input
          v-model="pet.name"
          type="text"
          class="form-input mt-1 block w-full"
          required
        />
      </div>
      <div class="mb-4">
        <label class="block text-gray-700">Categoría:</label>
        <select
          v-model="pet.category_id"
          class="form-select mt-1 block w-full"
          required
        >
          <option value="" disabled>Selecciona una categoría</option>
          <option
            v-for="category in categories"
            :key="category.id"
            :value="category.id"
          >
            {{ category.name }}
          </option>
        </select>
      </div>
      <div class="mb-4">
        <label class="block text-gray-700">Estado:</label>
        <select
          v-model="pet.status"
          class="form-select mt-1 block w-full"
          required
        >
          <option value="available">Disponible</option>
          <option value="pending">Pendiente</option>
          <option value="sold">Vendido</option>
        </select>
      </div>
      <div class="mb-4">
        <label class="block text-gray-700">Foto URL:</label>
        <input
          v-model="pet.photoUrls"
          type="text"
          class="form-input mt-1 block w-full"
          required
        />
      </div>
      <button
        type="submit"
        class="bg-blue-500 text-white px-4 py-2 rounded"
      >
        Actualizar
      </button>
    </form>
  </div>
</template>

<script>
export default {
  layout: 'custom',
  async asyncData({ $axios, params }) {
    try {
      const { data } = await $axios.get(`/pet/${params.id}`);
      return { pet: data };
    } catch (error) {
      console.error('Error fetching pet details:', error);
      return { pet: {} };
    }
  },
  data() {
    return {
      pet: {},
      categories: [] // Para almacenar las categorías
    };
  },
  async created() {
    // Obtener las categorías cuando el componente se crea
    try {
      const { data } = await this.$axios.get('/category');
      this.categories = data;
    } catch (error) {
      console.error('Error fetching categories:', error);
    }
  },
  methods: {
    async updatePet() {
      try {
        await this.$axios.put(`/pet/${this.pet.id}`, this.pet);
        this.$router.push('/');
      } catch (error) {
        console.error('Error updating pet:', error);
      }
    }
  }
};
</script>

<style scoped>
/* Estilos personalizados */
</style>
