<script setup lang="ts">
import { RouterLink, RouterView, useRoute, useRouter } from 'vue-router'
import { useMessageStore } from './stores/message';
import { storeToRefs } from 'pinia';
const store = useMessageStore()
const { message } = storeToRefs(store)
import { ref } from 'vue'

const pageSizes = [1, 2, 4, 6, 8, 10]
const pageSize = ref(pageSizes[1])
const router = useRouter()
const route = useRoute()

const updatePageSize = () => {
  router.push({ 
    name: 'event-list-view',
    query: { ...route.query, pageSize: pageSize.value, page: 1 }
  })
}

if (route.query.pageSize) {
  pageSize.value = parseInt(route.query.pageSize.toString())
}
</script>

<template>
  <div class="text-center font-sans text-gray-700 antialiased">
    <header>
      <div id="flashMessage" class="animate-fade" v-if="message">
        <h4>{{ message }}</h4>
      </div>
      <div class="wrapper">
        <nav class="py-6">
          <RouterLink class="font-bold text-gray-700" exact-active-class="text-green-500" :to="{ name: 'event-list-view', query: { pageSize: pageSize } }">Event</RouterLink>
          <RouterLink class="font-bold text-gray-700" exact-active-class="text-green-500" :to="{ name: 'about' }">About</RouterLink>
          <RouterLink :to="{ name: 'Student' }">Student</RouterLink>
        </nav>
        <div class="mt-4">
          <label class="mr-2" for="page-size">Event per page</label>
          <select class="mr-2" id="page-size" v-model="pageSize" @change="updatePageSize">
            <option  class="text-gray-700 hover:text-green-500" exact-active-class="text-green-500" v-for="size in pageSizes" :key="size" :value="size">{{ size }}</option>
          </select>
        </div>
      </div>
    </header>
    <RouterView />
  </div>
</template>
