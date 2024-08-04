<template>
  <table class="min-w-full bg-white">
    <thead>
      <tr>
        <th class="w-1/4 px-4 py-2">Categoría</th>
        <th class="w-1/4 px-4 py-2">Nombre</th>
        <th class="w-1/4 px-4 py-2">Estado</th>
        <th class="w-1/4 px-4 py-2">PhotoUrl</th>
        <th class="w-1/4 px-4 py-2">Acciones</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="pet in pets" :key="pet.id" class="bg-gray-100">
        <td class="border px-4 py-2">{{ categoryNames[pet.category_id] || 'Cargando...' }}</td>
        <td class="border px-4 py-2">{{ pet.name }}</td>
        <td class="border px-4 py-2">{{ pet.status }}</td>
        <td class="border px-4 py-2">
          <img :src="pet.photoUrls" alt="Photo" class="h-12 w-12 object-cover">
        </td>
        <td class="border px-4 py-2">
          <nuxt-link :to="`/edit/${pet.id}`" class="bg-blue-500 text-white px-4 py-2 rounded">Actualizar</nuxt-link>
        </td>
        <td class="border px-4 py-2">
          <button @click="deletePet(pet.id)" class="bg-red-500 text-white px-4 py-2 rounded">Eliminar</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: {
    pets: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      categoryNames: {} // Para almacenar los nombres de las categorías por ID
    };
  },
  async mounted() {
    // Cargar nombres de categorías para todos los IDs que aparezcan en pets
    const categoryIds = [...new Set(this.pets.map(pet => pet.category_id))];
    for (const id of categoryIds) {
      await this.fetchCategoryName(id);
    }
  },
  methods: {
    async fetchCategoryName(id) {
      try {
        const { data } = await this.$axios.get(`/category/${id}`);
        this.$set(this.categoryNames, id, data.name);
      } catch (error) {
        console.error('Error fetching category name:', error);
        
        this.$set(this.categoryNames, id, 'Desconocida');
      }
    },
    async deletePet(petId) {
      try {
        await this.$axios.delete(`/pet/${petId}`);
        this.$emit('pet-updated');
      } catch (error) {
        console.error('Error deleting pet:', error);
      }
    }
  }
};
</script>

<style scoped>
/* Estilos personalizados */
</style>
