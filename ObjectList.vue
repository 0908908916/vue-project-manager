<template>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
    <div v-for="item in filteredItems" :key="item.id"
         class="bg-gradient-to-br from-yellow-200 via-pink-200 to-purple-300 
                rounded-2xl shadow-lg p-5 w-full break-words overflow-hidden">
      <h2 class="text-xl md:text-2xl font-bold mb-2 text-purple-900 break-words truncate">
        {{ item.name }}
      </h2>
      <p class="text-gray-700 mb-4 break-words">
        {{ item.description }}
      </p>
      <div class="flex justify-end gap-2 flex-wrap">
        <button @click="$emit('edit', item)"
                class="bg-gradient-to-r from-indigo-400 to-purple-500 text-white px-3 py-1 rounded-lg hover:from-indigo-500 hover:to-purple-600 transition">
          編輯
        </button>
        <button @click="$emit('delete', item.id)"
                class="bg-gradient-to-r from-red-400 to-pink-500 text-white px-3 py-1 rounded-lg hover:from-red-500 hover:to-pink-600 transition">
          刪除
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  items: Array,
  searchText: String
})

const filteredItems = computed(() => {
  if (!props.searchText) return props.items
  return props.items.filter(i => i.name.toLowerCase().includes(props.searchText.toLowerCase()))
})
</script>
