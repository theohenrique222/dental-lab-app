<template>
    <section class="w-full px-10 flex">
      <div v-for="content in contents" :key="content.id"
        class="bg-blue-950 w-2/5 text-white p-4 rounded-lg shadow-lg text-center m-auto"
      >
        <div class="h-full m-auto py-10">
          <p class="text-lg uppercase font-bold">{{ content.title }}</p>
          <h2 class="text-2xl font-extrabold">{{ content.content }}</h2>
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
        categories: [],
        searchQuery: "",
        contents: []
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
  
          // Atualize os dados
          this.services = servicesResponse.data;
          this.categories = categoriesResponse.data;
  
          // Atualize o conteúdo dos widgets com os dados retornados
          this.contents = [
            { id: 1, title: 'Total de serviços', content: this.services.length },
            { id: 2, title: 'Total de categorias', content: this.categories.length }
          ];
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
  
  <style>
  </style>