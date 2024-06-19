
<template>
  <UNotifications />
  <h1 class="text-center text-3xl my-[2%]">Categories List</h1>
  <div class="mx-[3%] my-[2%]">
    <div class="flex justify-between items-center w-full px-4 py-3">
    </div>
    <UTable
        :rows="rows"
        :columns="columns"
        :loading-state="{ icon: 'i-heroicons-arrow-path-20-solid', label: 'Loading...' }"
        :progress="{ color: 'primary', animation: 'carousel' }">

      <template #actions-data="{ row }">
        <UDropdown :items="items(row)">
          <UButton color="gray" variant="ghost" icon="i-heroicons-ellipsis-horizontal-20-solid" />
        </UDropdown>
      </template>
    </UTable>
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


interface Category {
  id: number;
  title: string;
  parent_category: Category;
}
interface ParentCategory {
  title: string;
}

const columns = [
  { key: 'id', label: 'ID' },
  { key: 'title', label: 'Категорія' },
  { key: 'parent_category.title', label: 'ParentCategory' },
  { key: 'actions', label: 'Actions'}
];


const category = ref<Category[]>([]);

const getCategory = async () => {
  const response = await $fetch<Category[]>('http://localhost:8000/api/blog/categories');
  category.value = response;
};

getCategory();
const toast = useToast()
const items = (row: Category) => [
  [{
    label: 'Edit',
    icon: 'i-heroicons-pencil-square-20-solid',
    click: () => console.log('Edit', row.id)
  }, {
    label: 'Details',
    icon: 'i-heroicons-arrow-right-circle-20-solid',
    click: () => navigateTo(`/Categories/${row.id}`)
  }], [{
    label: 'Delete',
    icon: 'i-heroicons-trash-20-solid',
    click: () => {
      $fetch(`http://localhost:8000/api/blog/categories/delete/${row.id}`, {
        method: 'DELETE',
        body: {id: row.id}
      })
          .then(res => {
            console.log(res);
            toast.add({title: 'Success', description: "Category was deleted"})
            getCategory();
          })
          .catch(error => {
            console.error(error);
            alert('Не вдалось видалити категорію!');
          });
    }
  }]
]
const page = ref(1);
const pageCount = 5;

const totalRows = computed(() => category.value.length);

const rows = computed(() => {
  const startIndex = (page.value - 1) * pageCount;
  const endIndex = startIndex + pageCount;
  return category.value.slice(startIndex, endIndex);
});
</script>