<template>
  <div class=" mx-auto">
    <nav class="flex space-x-4 mb-4 m-4">
      <button
          :class="{ 'bg-gray-200': activeTab === 'blog', 'bg-white': activeTab !== 'blog' }"
          @click="activeTab = 'blog'"
          class="flex-1 w-full text-center py-2 px-4 rounded-lg text-gray-800 font-medium hover:bg-gray-300 focus:outline-none"
      >
        Blog Posts
      </button>
      <button
          :class="{ 'bg-gray-200': activeTab === 'categories', 'bg-white': activeTab !== 'categories'}"
          @click="activeTab = 'categories'"
          class="flex-1 w-full text-center py-2 px-4 rounded-lg text-gray-800 font-medium hover:bg-gray-300 focus:outline-none"
      >
        Categories
      </button>
    </nav>

    <div v-if="activeTab === 'blog'">
      <BlogPostsUi />
    </div>
    <div v-else-if="activeTab === 'categories'">
      <Categories />
    </div>
  </div>
</template>

<script setup lang="ts">
import BlogPostsUi from "~/pages/BlogPostsUi.vue";
import Categories from "~/pages/Categories/Categories.vue";
import { ref } from "vue";

const activeTab = ref('blog');
</script>
