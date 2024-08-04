<!-- pages/update.vue -->
<template>
    <div class="container mx-auto py-8">
      <h1 class="text-2xl font-bold mb-4">Actualizar Mascota</h1>
      <form @submit.prevent="updatePet" class="bg-white p-6 rounded shadow-md">
        <div class="mb-4">
          <label for="petId" class="block text-sm font-medium text-gray-700">ID de la Mascota</label>
          <input
            v-model="searchId"
            id="petId"
            type="number"
            required
            class="form-input mt-1 block w-full"
          />
        </div>
        <div class="mb-4">
          <label for="name" class="block text-sm font-medium text-gray-700">Nombre</label>
          <input
            v-model="pet.name"
            id="name"
            type="text"
            required
            class="form-input mt-1 block w-full"
          />
        </div>
        <div class="mb-4">
          <label for="status" class="block text-sm font-medium text-gray-700">Estado</label>
          <select
            v-model="pet.status"
            id="status"
            required
            class="form-select mt-1 block w-full"
          >
            <option value="available">Available</option>
            <option value="pending">Pending</option>
            <option value="sold">Sold</option>
          </select>
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
    layout:"custom",
    data() {
      return {
        searchId: '',
        pet: {
          name: '',
          status: ''
        }
      };
    },
    methods: {
      async updatePet() {
        if (!this.searchId) return;
  
        try {
          await this.$axios.post(`/pet/${this.searchId}`, {
            name: this.pet.name,
            status: this.pet.status
          });
          // Redirige a la página principal o muestra un mensaje de éxito
          this.$router.push('/');
        } catch (error) {
          console.error('Error updating pet:', error);
        }
      }
    },
     watch: {
      async searchId(newId) {
        if (!newId) return;
  
        try {
          const { data } = await this.$axios.get(`/pet/${newId}`);
          this.pet = data ? { name: data.name, status: data.status } : { name: '', status: '' };
        } catch (error) {
          console.error('Error fetching pet:', error);
          this.pet = { name: '', status: '' };
        }
      }
    }
  };
  </script>
  
  <style scoped>
  /* Estilos personalizados */
  </style>
  