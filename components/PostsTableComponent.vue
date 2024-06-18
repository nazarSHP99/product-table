<template>
  <div class=" flex justify-center">

    <div class="flex justify-center">

      <div class="w-full">

        <nav class="navbar bg-gray-100">

          <a href="/admin/blog/posts/create" class="">Додати</a>

        </nav>

        <div class="card">

          <div class="card-body">

            <table class="table table-auto">

              <thead>

              <tr>

                <th>#</th>

                <th>Автор</th>

                <th>Категорія</th>

                <th>Заголовок</th>

                <th>Дата публікації</th>
                <th>Деталі</th>

              </tr>

              </thead>

              <tbody>

              <tr v-for="post in posts">

                <td>{{post.id}}</td>

                <td>{{post.user.name}}</td>

                <td>{{post.category.title}}</td>

                <td><a :href="'/admin/blog/posts/' + post.id + '/edit'">{{post.title}}</a></td>

                <td>{{post.published_at}}

                </td>
                <td>
                  <router-link :to="`/BlogPostDetail/${post.id}`">Detail</router-link>
                </td>

              </tr>

              </tbody>

            </table>

          </div>

        </div>

      </div>

    </div>

  </div>

</template>



<script setup lang="ts">
interface Post {
  id: number;
  title: string;
  published_at: string;
  user: User,
  category: Category
}
interface User{
  name: string;
}
interface Category{
  title: string;
}
const posts = ref<Post[]>([]);

const getPosts = async () => {
  $fetch<Post[]>('http://localhost:8000/api/blog/posts')
      .then(response => {
        console.log(response);
        posts.value = response;

      });
};

getPosts();

</script>



<style scoped>
.container {
  width: 100%; /* Замість max-width */
  margin: 0 auto;
  padding: 0 20px;
  box-sizing: border-box;
}

.navbar {
  width: 100%;
  margin-bottom: 20px;
  padding: 10px 20px;
  box-sizing: border-box;
  background-color: #1a1a1a;
  border-radius: 8px;
  display: flex;
  justify-content: flex-end;
}

.card {
  width: 100%;
  margin: 0 auto;
  background-color: #2a2a2a;
  border-radius: 8px;
}

.card-body {
  padding: 20px;
}

.search-input {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  border: 1px solid #444;
  border-radius: 4px;
  background-color: #3a3a3a;
  color: white;
  font-size: 16px;
}

.table-responsive {
  overflow-x: auto;
}
.table {
  width: 100%;
  border-collapse: collapse;
  background-color: #3a3a3a;
  box-sizing: border-box;
  border-radius: 8px;
  overflow: hidden;
}

.table th,
.table td {
  padding: 12px 10px;
  border-bottom: 1px solid #555;
  text-align: left;
  color: white;
}

.table th {
  background-color: #4a4a4a;
  font-weight: bold;
}

.table td a {
  color: #66bfff;
  text-decoration: none;
}

.table td a:hover {
  text-decoration: underline;
}

.pagination {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.pagination button {
  margin: 0 5px;
  padding: 8px 16px;
  border: 1px solid #444;
  background-color: #333;
  color: white;
  cursor: pointer;
  border-radius: 4px;
  transition: background-color 0.2s;
}

.pagination button:hover {
  background-color: #555;
}

.pagination button.active {
  background-color: #007bff;
  color: white;
}

.pagination button:disabled {
  background-color: #444;
  cursor: not-allowed;
}
</style>