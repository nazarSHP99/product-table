<template>
  <div v-if="post">
    <div class="flex center mb-4">
    </div>

    <div v-if="activeTab === 'Details'">
      <div class="flex justify-center">
        <div class="w-full md:w-2/3">
          <div class="bg-slate-300 shadow-md rounded-lg p-4">
            <h3 class="text-xl text-slate-950 font-bold mb-4">Post Detail</h3>
            <button
                v-for="tab in tabs"
                :key="tab"
                :class="['px-3 py-1 mr-2 rounded focus:outline-none', activeTab === tab ? 'bg-blue-500 text-white' : 'bg-gray-200']"
                @click="activeTab = tab"
            >
              {{ tab }}
            </button>
            <div v-if="post && post.is_published" class="text-green-500 mb-4 bg-slate-700 rounded p-2 mt-0.5">
              Опубліковано {{formatDate(post.published_at)}}
            </div>
            <div v-else class="text-red-500 mb-4">
              Чернетка
            </div>
            <div class="form-group mb-4">
              <label for="title" class="block text-slate-950 font-bold">Title</label>
              <input type="text" class="w-full p-2 border rounded bg-slate-700" id="title" v-model="post.title" readonly>
            </div>
            <div class="form-group mb-4">
              <label for="user" class="block text-slate-950 font-bold">User</label>
              <input type="text" class="w-full p-2 border rounded bg-slate-700" id="user" v-model="post.user.name" readonly>
            </div>
            <div class="form-group mb-4">
              <h3 class="text-xl font-bold mb-4 text-slate-950">Post Content</h3>
              <textarea class="w-full p-2 border rounded bg-slate-700" id="content" v-model="post.content_raw" readonly rows="5"></textarea>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div v-if="activeTab === 'Content'">
      <div class="flex justify-center">
        <div class="w-full md:w-2/3">
          <div class="bg-gray-800 shadow-md rounded-lg p-4">
            <h3 class="text-xl font-bold mb-4">More detail</h3>
            <button
                v-for="tab in tabs"
                :key="tab"
                :class="['px-3 py-1 mr-2 rounded focus:outline-none', activeTab === tab ? 'bg-blue-500 text-white' : 'bg-gray-200']"
                @click="activeTab = tab"
            >
              {{ tab }}
            </button>
            <div>
              <p class="text-green-500 bg-slate-700 rounded p-2 mt-0.5">Created at {{ formatDate(post.created_at) }}</p>
              <p class="text-yellow-500 bg-slate-700 rounded p-2 mt-0.5">Updated at {{ formatDate(post.updated_at) }}</p>
            </div>
            <div class="form-group mb-4">
              <label for="excerpt" class="block text-gray-100">Короткий текст</label>
              <input type="text" class="w-full p-2 border rounded bg-slate-700" id="excerpt" v-model="post.excerpt" readonly>
            </div>
            <div class="form-group mb-4">
              <label for="slug" class="block text-gray-100">Псевдонім</label>
              <input type="text" class="w-full p-2 border rounded bg-slate-700" id="slug" v-model="post.slug" readonly>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-else>Loading...</div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useRoute } from 'vue-router';
import { format } from 'date-fns';

const route = useRoute();
const id = Number(route.params.id);

interface Post {
  id: number;
  title: string;
  content_raw: string;
  published_at: string;
  user: User;
  category: Category;
  is_published: boolean;
  excerpt: string;
  slug: string;
  created_at: string;
  updated_at: string;
  deleted_at: string;

}

interface User {
  name: string;
}

interface Category {
  title: string;
}

const post = ref<Post>();
const tabs = ref(['Details', 'Content']);
const activeTab = ref('Details');

const getPost = async (id: number) => {
  const response = await $fetch<Post>(`http://localhost:8000/api/blog/posts/${id}`);
  post.value = response;
};
const formatDate = (dateString: string) => {
  return format(new Date(dateString), 'dd-MM-yyyy HH:mm');
};

getPost(id);
</script>