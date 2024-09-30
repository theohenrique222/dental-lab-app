<template>
  <section>
    <Navbard/>
    <main class="bg-neutral-900 min-h-screen">
      <div class="container mx-auto px-4 sm:px-8">
        <div class="py-4">
          <div class="mt-10">
            <h1 class="text-4xl font-extrabold text-white mb-8 text-center">
              Tabela de Preços
            </h1>
          </div>

          <div class="w-full justify-center flex p-5">
            <div class="relative w-2/3">
              <input
                type="text"
                v-model="searchQuery"
                placeholder="Buscar"
                class="w-full p-3 pl-10 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring focus:border-blue-300"
              />
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                class="w-5 h-5 absolute top-1/2 left-3 transform -translate-y-1/2 text-gray-400"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="m21 21-5.197-5.197m0 0A7.5 7.5 0 1 0 5.196 5.196a7.5 7.5 0 0 0 10.607 10.607Z"
                />
              </svg>
            </div>
            <button class="bg-blue-500 text-white px-8 rounded-md mx-1 font-semibold hover:bg-blue-950 transition-all">Buscar</button>
          </div>

          <div
            v-for="(services, category) in groupedServices"
            :key="category"
            class="min-w-full mb-2 shadow-lg rounded-lg overflow-hidden"
          >
            <details class="bg-blue-900 p-4 w-full flex flex-col justify-center">
              <summary class="text-xl font-bold text-white uppercase cursor-pointer">
                {{ category }}
              </summary>
              <table class="min-w-full leading-normal bg-white rounded-lg mt-4">
                <thead>
                  <tr class="bg-blue-100">
                    <th
                      v-for="table in tables"
                      :key="table.id"
                      class="px-5 py-3 text-center border-b-2 border-gray-200 text-xs font-semibold text-gray-600 uppercase tracking-wider"
                    >
                      {{ table.title }}
                    </th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    v-for="(item, index) in services"
                    :key="index"
                    class="hover:bg-gray-50 transition-colors duration-200 text-center"
                  >
                    <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm border-r">
                      <p class="text-gray-900 whitespace-no-wrap">{{ item.name }}</p>
                    </td>
                    <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm border-r">
                      <p class="text-gray-600 whitespace-no-wrap">{{ item.description }}</p>
                    </td>
                    <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm border-r">
                      <p class="text-gray-900 whitespace-no-wrap">{{ item.price }} R$</p>
                    </td>
                  </tr>
                </tbody>
              </table>
            </details>
          </div>
        </div>
      </div>
    </main>
  </section>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      services: [],
      categories: [],
      searchQuery: "",
      tables: [
        { id: 1, title: "Nome" },
        { id: 2, title: "Descrição" },
        { id: 3, title: "Valor" },
      ],
    };
  },
  computed: {
    filteredServices() {
      return this.services.filter((service) => {
        const searchLower = this.searchQuery.toLowerCase();
        return (
          service.name.toLowerCase().includes(searchLower) ||
          service.description.toLowerCase().includes(searchLower)
        );
      });
    },
    groupedServices() {
      return this.filteredServices.reduce((groups, service) => {
        const categoryName = this.getCategoryName(service.category_id);
        if (!groups[categoryName]) {
          groups[categoryName] = [];
        }
        groups[categoryName].push(service);
        return groups;
      }, {});
    },
  },
  methods: {
    async fetchData() {
      try {
        const [servicesResponse, categoriesResponse] = await Promise.all([
          axios.get("http://dental-lab.test/api/services"),
          axios.get("http://dental-lab.test/api/category"),
        ]);
        this.services = servicesResponse.data;
        this.categories = categoriesResponse.data;
      } catch (error) {
        console.error("Erro ao buscar os dados:", error);
      }
    },

    getCategoryName(categoryId) {
      const category = this.categories.find((cat) => cat.id === categoryId);
      return category ? category.category : "Desconhecida";
    },
  },
  mounted() {
    this.fetchData();
  },
};
</script>