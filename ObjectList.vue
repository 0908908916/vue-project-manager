<script setup>
import { ref, onMounted, watch } from 'vue'

const objects = ref([])
const newObject = ref('')

// 頁面載入時，從 localStorage 讀取資料
onMounted(() => {
  const saved = localStorage.getItem('objects')
  if (saved) {
    objects.value = JSON.parse(saved)
  }
})

// 監聽 objects，資料變動時存回 localStorage
watch(objects, (newVal) => {
  localStorage.setItem('objects', JSON.stringify(newVal))
}, { deep: true })

// 新增物件
const addObject = () => {
  if (newObject.value.trim() === '') return
  objects.value.push({ id: Date.now(), name: newObject.value })
  newObject.value = ''
}

// 刪除物件
const removeObject = (id) => {
  objects.value = objects.value.filter(obj => obj.id !== id)
}
</script>

<template>
  <div class="max-w-md mx-auto bg-white shadow-md rounded p-4">
    <h1 class="text-xl font-bold mb-4">📦 物件列表</h1>

    <!-- 新增物件 -->
    <div class="flex gap-2 mb-4">
      <input
        v-model="newObject"
        type="text"
        placeholder="輸入物件名稱"
        class="border rounded px-2 py-1 flex-1"
      />
      <button
        @click="addObject"
        class="bg-blue-500 text-white px-3 py-1 rounded hover:bg-blue-600"
      >
        新增
      </button>
    </div>

    <!-- 物件列表 -->
    <ul>
      <li
        v-for="obj in objects"
        :key="obj.id"
        class="flex justify-between items-center bg-gray-100 p-2 rounded mb-2"
      >
        <span>{{ obj.name }}</span>
        <button
          @click="removeObject(obj.id)"
          class="text-red-500 hover:text-red-700"
        >
          ❌
        </button>
      </li>
    </ul>

    <!-- 沒有物件時 -->
    <p v-if="objects.length === 0" class="text-gray-500 text-sm text-center">
      尚未新增任何物件
    </p>
  </div>
</template>
