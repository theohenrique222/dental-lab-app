<template>
  <nav class="fixed w-full bg-white/10 border-b-2 border-white/30 flex items-center backdrop-blur-xl">
    <div class="flex items-center justify-between w-full max-w-6xl m-auto">
      <div class="flex">
        <a href="" class="text-5xl font-extrabold text-white">DL</a>
      </div>
      <ul class="flex space-x-3">
        <li v-for="(item, index) in menuItems" :key="index">
          <a href="" class="text-white p-2 hover:border-b-2 hover:border-b-blue-500 transition-all">
            {{ item.name }}
          </a>
        </li>
        <li>
          <AutoComplete v-model="value" dropdown :suggestions="items" @complete="search" />
        </li>
      </ul>
    </div>
  </nav>
</template>

<script>
import AutoComplete from 'primevue/autocomplete';

export default {
  components: {
    // AutoComplete
  },
  data() {
    return {
      menuItems: [
        { name: 'Inicio', link: '#' },
        { name: 'Serviços', link: '/services' },
        { name: 'Categorias', link: '/category' },
      ]
    }
  },
}
</script>

<style></style>