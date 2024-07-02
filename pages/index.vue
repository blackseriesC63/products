<script setup>
import { ref, onMounted, computed } from "vue";
import axios from "axios";

const data = ref([]);
const searchQuery = ref("");

const fetchProducts = async () => {
  try {
    const response = await axios.get("https://dummyjson.com/products");
    return response.data.products;
  } catch (error) {
    console.error("Error fetching products:", error);
    return [];
  }
};

const filteredProducts = computed(() => {
  const query = searchQuery.value.toLowerCase();
  return data.value.filter(
    (product) => product.title.toLowerCase().includes(query) || product.description.toLowerCase().includes(query)
  );
});

onMounted(async () => {
  const products = await fetchProducts();
  data.value = products;
});
</script>

<template>
  <div>
    <div class="flex justify-between items-center py-4">
      <div class="flex items-center border border-[#F3F3F3] rounded-xl p-2">
        <span
          class="icon-[ic--baseline-search] px-4"
          style="width: 24px; height: 24px; color: #9b9b9b"
        ></span>
        <input
          type="text"
          class="outline-none px-2"
          v-model="searchQuery"
          placeholder="Search products..."
        />
      </div>
    </div>
    <div class="grid grid-cols-4 gap-5">
      <div
        v-for="product in filteredProducts"
        :key="product.id"
        class="border p-4 rounded"
      >
        <img :src="product.thumbnail" :alt="product.title" class="w-full h-auto"/>
        <h4 class="font-bold">{{ product.title }}</h4>
        <p>{{ product.description }}</p>
        <p>{{ product.price }} USD</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Add any scoped styles here if needed */
</style>
