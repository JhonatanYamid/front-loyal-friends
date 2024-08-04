<template>
    <div class="p-4 border border-gray-300 rounded">
      <h2 class="text-xl font-semibold mb-4">Actualizar Mascota</h2>
      <form @submit.prevent="updatePet">
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
        <button
          type="button"
          @click="cancel"
          class="bg-gray-500 text-white px-4 py-2 rounded ml-2"
        >
          Cancelar
        </button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      pet: {
        type: Object,
        required: true
      }
    },
    methods: {
      async updatePet() {
        try {
          await this.$axios.post(`/pet/${this.pet.id}`, {
            name: this.pet.name,
            status: this.pet.status
          });
          this.$emit('updated');
        } catch (error) {
          console.error('Error updating pet:', error);
        }
      },
      cancel() {
        this.$emit('cancel');
      }
    }
  };
  </script>
  
  <style scoped>
  /* Estilos personalizados */
  </style>
  