<template>
  <section class="bg-neutral-200">
    <div class="container mx-auto px-4 sm:px-8">
      <div class="py-4">
        <h1 class="text-4xl font-extrabold text-gray-800 mb-8 text-center">
          Tabela de Preço
        </h1>
        
        <div class="mb-6">
          <input 
            type="text" 
            v-model="searchQuery" 
            placeholder="Buscar" 
            class="w-1/2 p-3 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring focus:border-blue-300"
          />
        </div>

        <div class="min-w-full shadow-lg rounded-lg overflow-hidden">
          <table class="min-w-full leading-normal bg-white rounded-lg">
            <thead>
              <tr class="bg-gray-100">
                <th v-for="table in tables" :key="table.id"
                  class="px-5 py-3 border-b-2 border-gray-200 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider"
                >
                  {{ table.title }}
                </th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="(item, index) in filteredServices"
                :key="index"
                class="hover:bg-gray-50 transition-colors duration-200"
              >
                <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                  <p class="text-gray-900 whitespace-no-wrap">
                    {{ item.name }}
                  </p>
                </td>
                <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                  <p class="text-gray-600 whitespace-no-wrap">
                    {{ item.description }}
                  </p>
                </td>
                <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                  <p class="text-gray-900 whitespace-no-wrap">
                    R$ {{ item.price }}
                  </p>
                </td>
                <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                  <p class="text-gray-600 whitespace-no-wrap">
                    {{ formatDate(item.updated_at) }}
                  </p>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </section>
</template>
  
<script>
import axios from "axios";

export default {
  data() {
    return {
      services: [],
      searchQuery: "", 
      tables: [
        { id: 1, title: "Nome" },
        { id: 2, title: "Descrição" },
        { id: 3, title: "Valor" },
        { id: 4, title: "Última atualização" },
      ]
    };
  },
  computed: {
    filteredServices() {
      return this.services.filter(service => {
        const searchLower = this.searchQuery.toLowerCase();
        return (
          service.name.toLowerCase().includes(searchLower) ||
          service.description.toLowerCase().includes(searchLower)
        );
      });
    }
  },
  methods: {
    fetchServices() {
      axios
        .get("http://dental-lab.test/api/services")
        .then((response) => {
          this.services = response.data;
        })
        .catch((error) => {
          console.error("Erro ao buscar os dados:", error);
        });
    },
    formatDate(date) {
      const options = { year: "numeric", month: "long", day: "numeric" };
      return new Date(date).toLocaleDateString("pt-BR", options);
    },
  },
  mounted() {
    this.fetchServices();
  },
};
</script>