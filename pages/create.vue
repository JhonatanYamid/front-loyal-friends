<template>
    <div class="container mx-auto py-8">
      <h1 class="text-2xl font-bold mb-4">Crear Nueva Mascota</h1>
      <!-- Formulario para crear una nueva mascota -->
      <form @submit.prevent="createPet">
        <div class="mb-4">
          <label for="name" class="block text-gray-700">Nombre:</label>
          <input
            type="text"
            v-model="name"
            id="name"
            class="mt-1 block w-full border-gray-300 rounded-md shadow-sm"
          />
        </div>
        <div class="mb-4">
          <label for="category_id" class="block text-gray-700">Categoría:</label>
          <select
            v-model="category_id"
            id="category_id"
            class="mt-1 block w-full border-gray-300 rounded-md shadow-sm"
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
          <label for="photoUrls" class="block text-gray-700">Photo URL:</label>
          <input
            type="text"
            v-model="photoUrls"
            id="photoUrls"
            class="mt-1 block w-full border-gray-300 rounded-md shadow-sm"
          />
        </div>
        <div class="mb-4">
          <label for="status" class="block text-gray-700">Estado:</label>
          <select
            v-model="status"
            id="status"
            class="mt-1 block w-full border-gray-300 rounded-md shadow-sm"
          >
            <option value="available">Disponible</option>
            <option value="pending">Pendiente</option>
            <option value="sold">Vendido</option>
          </select>
        </div>
        <button
          type="submit"
          class="bg-blue-500 text-white px-4 py-2 rounded"
        >
          Crear
        </button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    layout: 'custom',
    data() {
      return {
        name: '',
        category_id: '',
        photoUrls: '',
        status: '',
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
      async createPet() {
        try {
          await this.$axios.post('/pet', {
            name: this.name,
            category_id: this.category_id,
            photoUrls: this.photoUrls,
            status: this.status
          });
          this.$router.push('/');
        } catch (error) {
          console.error('Error creating pet:', error);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  /* Estilos personalizados */
  </style>
  