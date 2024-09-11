<template>
  <section class="bg-neutral-200">
    <div class="container mx-auto px-4 sm:px-8">
      <div class="py-4">
        <h1 class="text-4xl font-extrabold text-gray-800 mb-8 text-center">
          Tabela de Preço
        </h1>
        <div class="min-w-full shadow-lg rounded-lg overflow-hidden">
          <table class="min-w-full leading-normal bg-white rounded-lg">
            <thead>
              <tr class="bg-gray-100">
                <th
                  class="px-5 py-3 border-b-2 border-gray-200 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider"
                >
                  Nome
                </th>
                <th
                  class="px-5 py-3 border-b-2 border-gray-200 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider"
                >
                  Descrição
                </th>
                <th
                  class="px-5 py-3 border-b-2 border-gray-200 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider"
                >
                  Preço
                </th>
                <th
                  class="px-5 py-3 border-b-2 border-gray-200 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider"
                >
                  Última Atualização
                </th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="(item, index) in services"
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
    };
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