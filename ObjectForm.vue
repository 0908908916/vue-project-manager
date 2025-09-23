<template>
  <div class="bg-gradient-to-r from-blue-100 via-purple-100 to-pink-100 p-6 rounded-2xl shadow-lg max-w-md mx-auto">
    <h2 class="text-2xl md:text-3xl font-extrabold mb-4 text-purple-900">
      {{ editObj ? '編輯物件' : '新增物件' }}
    </h2>
    <form @submit.prevent="handleSubmit" class="space-y-4">
      <div>
        <label class="block mb-1 font-medium text-purple-800">名稱</label>
        <input v-model="name" type="text" placeholder="輸入名稱"
               class="w-full border border-purple-300 px-3 py-2 rounded-lg focus:outline-none focus:ring-4 focus:ring-pink-300 transition" />
      </div>
      <div>
        <label class="block mb-1 font-medium text-purple-800">描述</label>
        <textarea v-model="description" placeholder="輸入描述"
                  class="w-full border border-purple-300 px-3 py-2 rounded-lg focus:outline-none focus:ring-4 focus:ring-pink-300 transition"></textarea>
      </div>
      <div class="flex justify-end gap-2 flex-wrap">
        <button type="button" @click="$emit('cancel')"
                class="bg-gray-300 px-4 py-2 rounded-lg hover:bg-gray-400 transition">
          取消
        </button>
        <button type="submit"
                class="bg-gradient-to-r from-green-400 to-blue-500 px-4 py-2 text-white rounded-lg hover:from-green-500 hover:to-blue-600 transition">
          {{ editObj ? '更新' : '新增' }}
        </button>
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  editObj: Object
})
const emit = defineEmits(['cancel', 'save'])

const name = ref('')
const description = ref('')

watch(() => props.editObj, (val) => {
  if (val) {
    name.value = val.name
    description.value = val.description
  } else {
    name.value = ''
    description.value = ''
  }
})

const handleSubmit = () => {
  if (!name.value.trim()) return alert('名稱不能為空')
  if (!description.value.trim()) return alert('描述不能為空')
  emit('save', {
    id: props.editObj ? props.editObj.id : null,
    name: name.value,
    description: description.value
  })
}
</script>
