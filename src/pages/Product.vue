<template>
    <Header />
    <div class="container mx-auto py-8">
        <div v-if="product">
            <div class="flex flex-col lg:flex-row items-center">
                <div class="flex-1 mb-8 lg:mb-0 lg:w-1/2">
                    <img :src="product.image" :alt="product.name"
                        class="w-full h-auto object-cover rounded-lg shadow-md" />
                </div>

                <div class="flex-1 lg:ml-8">
                    <h1 class="text-3xl font-bold text-gray-800 mb-4">{{ product.name }}</h1>
                    <p class="text-gray-600 mb-6">{{ product.description }}</p>

                    <div class="bg-gray-100 p-4 rounded-lg shadow-md mb-6">
                        <h2 class="text-lg font-semibold text-gray-700 mb-2">Характеристики:</h2>
                        <ul class="list-disc list-inside text-gray-600">
                            <li v-for="(spec, index) in product.specs" :key="index">
                                {{ spec }}
                            </li>
                        </ul>
                    </div>

                    <div class="text-xl font-semibold text-gray-800 mb-6">
                        {{ formattedPrice(product.price) }} ₽
                    </div>

                    <button @click="addToCart(product)"
                        class="px-6 py-3 bg-gray-600 text-white font-semibold rounded-lg hover:bg-gray-700 transition duration-300">
                        Добавить в корзину
                    </button>
                </div>
            </div>
        </div>
        <div v-else>
            <p>Товар не найден.</p>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Header from "@/components/Header.vue";
import { useCartStore } from "@/store/cartStore";
import mondeo from '@/assets/mondeo.png';
import logan from '@/assets/logan.png';
import solaris from '@/assets/solaris.png';
import avensis from '@/assets/avensis.png';
import megane from '@/assets/megane.png';

interface Product {
    id: number;
    name: string;
    description: string;
    price: number;
    image: string;
    specs: string[];
}

export default defineComponent({
    name: "Product",
    components: { Header },
    data() {
        return {
            product: null as Product | null,
        };
    },
    mounted() {
        this.fetchProduct();
    },
    methods: {
        fetchProduct(): void {
            const products: Product[] = [
                {
                    id: 1,
                    name: "Ford Mondeo",
                    description: "Новое поступление в Автосалон CAR CITY! Carcity45.ru",
                    price: 500000,
                    image: mondeo,
                    specs: [
                    ]
                },
                {
                    id: 2,
                    name: "Renault Logan",
                    description: "Новое поступление в Автосалон CAR CITY! Carcity45.ru",
                    price: 1000000,
                    image: logan,
                    specs: [
                    ]
                },
                {
                    id: 3,
                    name: "Hyundai Solaris",
                    description: "Новое поступление в Автосалон CAR CITY! Carcity45.ru",
                    price: 1500000,
                    image: solaris,
                    specs: [
                    ]
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
                    specs: [
                    ]
                }
            ];

            const paramId = Array.isArray(this.$route.params.id)
                ? this.$route.params.id[0]
                : this.$route.params.id;

            // Преобразуем параметр id в число
            const productId = parseInt(paramId, 10);

            if (isNaN(productId)) {
                console.error("Некорректный ID товара!");
                return;
            }

            this.product = products.find((prod) => prod.id === productId) || null;

            if (!this.product) {
                console.error("Товар с таким id не найден!");
            }
        },

        formattedPrice(price: number): string {
            return price.toLocaleString("ru-RU");
        },
        addToCart(product: Product): void {
            const cartStore = useCartStore();
            cartStore.addToCart(product);
        },
    },
});
</script>
