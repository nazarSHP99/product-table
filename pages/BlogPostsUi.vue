<template>
  <h1 class="text-center text-3xl my-[2%]">BlogPosts List</h1>
  <div class="mx-[3%] my-[2%]">
    <div class="flex justify-between items-center w-full px-4 py-3">
    </div>
    <UTable
        :rows="rows"
        :columns="columns"
        :loading-state="{ icon: 'i-heroicons-arrow-path-20-solid', label: 'Loading...' }"
        :progress="{ color: 'primary', animation: 'carousel' }"
    />
  </div>
  <div class="flex justify-end px-3 py-3.5 border-t border-gray-200 dark:border-gray-700">
    <UPagination
        v-model="page"
        :page-count="pageCount"
        :total="totalRows"
    />
  </div>

</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

interface Post {
  id: number;
  title: string;
  published_at: string;
  user: User;
  category: Category;
}

interface User {
  name: string;
}

interface Category {
  title: string;
}

const columns = [
  { key: 'id', label: 'ID' },
  { key: 'title', label: 'Title' },
  { key: 'published_at', label: 'Published At' },
  { key: 'user.name', label: 'User' },
  { key: 'category.title', label: 'Category' }
];

const posts = ref<Post[]>([]);

const getPosts = async () => {
  const response = await $fetch<Post[]>('http://localhost:8000/api/blog/posts');
  posts.value = response;
};

getPosts();

const page = ref(1);
const pageCount = 5;

const totalRows = computed(() => posts.value.length);

const rows = computed(() => {
  const startIndex = (page.value - 1) * pageCount;
  const endIndex = startIndex + pageCount;
  return posts.value.slice(startIndex, endIndex);
});
</script>