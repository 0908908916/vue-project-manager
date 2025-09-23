<template>
  <div class="min-h-screen bg-gradient-to-r from-purple-400 via-pink-400 to-red-400 p-4">
    <div class="max-w-5xl mx-auto bg-white/90 shadow-2xl rounded-2xl p-6 md:p-8 backdrop-blur-sm">
      <h1 class="text-3xl md:text-4xl font-extrabold mb-6 text-center text-purple-900 drop-shadow-lg">
        🌟 物件管理系統
      </h1>

      <div class="flex flex-col md:flex-row justify-between items-center mb-6 gap-4">
        <button @click="showForm = true; editObj = null"
                class="bg-gradient-to-r from-green-400 to-blue-500 text-white px-5 py-2 rounded-xl shadow-lg hover:from-green-500 hover:to-blue-600 transition w-full md:w-auto text-center">
          新增物件
        </button>
        <input type="text" placeholder="搜尋物件..." v-model="searchText"
               class="border border-gray-300 px-3 py-2 rounded-xl w-full md:w-1/3 focus:outline-none focus:ring-4 focus:ring-pink-300 transition" />
      </div>

      <ObjectForm v-if="showForm" :editObj="editObj" @cancel="showForm = false" @save="handleSave"/>
      <ObjectList v-else :items="items" :searchText="searchText" @edit="handleEdit" @delete="handleDelete"/>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ObjectList from './components/ObjectList.vue'
import ObjectForm from './components/ObjectForm.vue'

const showForm = ref(false)
const editObj = ref(null)
const searchText = ref('')

const items = ref([
  { id: 1, name: '物件 A', description: '這是物件 A 的描述' },
  { id: 2, name: '物件 B', description: '這是物件 B 的描述' },
  { id: 3, name: '物件 C', description: '這是物件 C 的描述' },
])

const handleEdit = (obj) => {
  editObj.value = obj
  showForm.value = true
}

const handleDelete = (id) => {
  items.value = items.value.filter(i => i.id !== id)
}

const handleSave = (obj) => {
  if (obj.id) {
    const index = items.value.findIndex(i => i.id === obj.id)
    if (index !== -1) items.value[index] = obj
  } else {
    obj.id = Date.now()
    items.value.push(obj)
  }
  showForm.value = false
}
</script>
