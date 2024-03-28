<script setup lang="ts">
const columns = [{
  key: 'title',
  label: 'Title',
  sortable: true
}, {
  key: 'description',
  label: 'Description',
  sortable: true
}, {
  key: 'price',
  label: 'Price',
  sortable: true
}, {
  key: 'rating',
  label: 'Rating',
  sortable: true
},{
  key: 'brand',
  label: 'Brand',
  sortable: true
},{
  key: 'category',
  label: 'Category',
  sortable: true
},{
  key: 'thumbnail',
  label: 'Thumbnail',
  sortable: true
}]
const { data } = await useAsyncData<any>(() => $fetch ('https://dummyjson.com/products'))
const products = data.value.products

const q = ref('')

const filteredRows = computed(() => {
  if (!q.value) {
    return products.slice((page.value - 1) * pageCount, (page.value) * pageCount)
  }

  return products.filter((product: any) => {
    return Object.values(product).some((value) => {
      return String(value).toLowerCase().includes(q.value.toLowerCase())
    })
  })
})
const page = ref(1)
const pageCount = 5

</script>

<template>
  <div class="flex px-3 py-3.5 border-b border-gray-200 dark:border-gray-700">
    <UInput v-model="q" placeholder="Filter products..." />
  </div>
  <div>
    <UTable :columns="columns" :rows="filteredRows"
    :ui="{td: {base: 'max-w-[0] truncate'}}">
      <template #rating-data="{ row }">
        <span :style="{ color: row.rating > 4.5 ? 'green' : 'red' }">{{ row.rating }}</span>
      </template>
      <template #thumbnail-data="{ row }" >
        <img :src="row.thumbnail" alt="Thumbnail" class="w-[100px] h-[100px]"/>
      </template>

    </UTable>
    <div class="flex justify-end px-3 py-3.5 border-t border-gray-200 dark:border-gray-700">
      <UPagination v-model="page" :page-count="pageCount" :total="products.length" />
    </div>

  </div>
</template>

<style scoped>

</style>