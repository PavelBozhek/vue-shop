<template>
  <div class="container mx-auto p-4">
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
      <div v-for="product in products" :key="product.id"
        class="bg-white shadow rounded-lg p-4 flex flex-col items-center transform transition-all duration-300 hover:scale-105 hover:shadow-xl">
        <img :src="product.image" :alt="product.name" class="w-32 h-32 object-cover mb-4 rounded-md"
          @click="goToProduct(product)" />
        <h2 class="text-lg font-semibold mb-2">{{ product.name }}</h2>
        <p class="text-gray-500 text-sm mb-2">{{ product.description }}</p>
        <span class="text-gray-800 font-bold text-lg">
          {{ formatPrice(product.price) }} ₽
        </span>
        <div class="flex space-x-2 mt-4">
          <button @click="goToProduct(product)"
            class="bg-gray-500 text-white px-4 py-2 rounded-md hover:bg-gray-600 transition w-full">
            Подробнее
          </button>
          <button @click="addToCart(product)"
            class="bg-gray-500 text-white px-4 py-2 rounded-md hover:bg-gray-600 transition w-full">
            В корзину
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { useCartStore } from '@/store/cartStore';
import mondeo from '@/assets/mondeo.png';
import logan from '@/assets/logan.png';
import solaris from '@/assets/solaris.png';
import avensis from '@/assets/avensis.png';
import megane from '@/assets/megane.png';



export default defineComponent({
  name: 'ProductCatalog',
  data() {
    return {
      products: [
        {
          id: 1,
          name: "Ford Mondeo",
          description: "Новое поступление в Автосалон CAR CITY! Carcity45.ru",
          price: 500000,
          image: mondeo,
        },
        {
          id: 2,
          name: "Renault Logan",
          description: "Новое поступление в Автосалон CAR CITY! Carcity45.ru",
          price: 1000000,
          image: logan,
        },
        {
          id: 3,
          name: "Hyundai Solaris",
          description: "Новое поступление в Автосалон CAR CITY! Carcity45.ru",
          price: 1500000,
          image: solaris,
        },
        {
          id: 4,
          name: "Toyota Avensis",
          description: "Новое поступление в Автосалон CAR CITY! Carcity45.ru",
          price: 25000000,
          image: avensis,
          specs: [
          ]
        },
        {
          id: 5,
          name: "Renault Megane",
          description: "Новое поступление в Автосалон CAR CITY! Carcity45.ru",
          price: 100,
          image: megane,
        }
      ],
    };
  },
  methods: {
    addToCart(product: { id: number, name: string, description: string, price: number, image: string }) {
      const cartStore = useCartStore();
      console.log(product)
      cartStore.addToCart(product);
    },
    formatPrice(price: number): string {
      return price.toLocaleString('ru-RU');
    },
    goToProduct(product: { id: number }) {
      this.$router.push(`/product/${product.id}`);
    },
  },
});
</script>