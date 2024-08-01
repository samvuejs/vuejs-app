<template>
  <h1>Hello world</h1>
  <div v-if="loading">
      <h4>Loading...</h4>
  </div>

  <div v-else>
      <div v-for="task in tasks" :key="task.$id">
              <h2>{{ task.title }}</h2>
      </div>
  </div>
</template>

<script setup>
import { Client, Databases } from "appwrite";
import { onMounted, ref } from "vue";

const tasks = ref([])
const loading = ref(false)

const client = new Client();

client
  .setEndpoint(import.meta.env.VITE_ENDPOINT)
  .setProject(import.meta.env.VITE_PROJECT);

const databases = new Databases(client);

onMounted(() => {
  const loadTasks = async () => {
    loading.value = true
    const result = await databases.listDocuments(
      import.meta.env.VITE_DB_ID, // databaseId
      import.meta.env.VITE_COLLECTION_ID, // collectionId
    );
   console.log(result)
   tasks.value = result.documents
   loading.value = false
  };
  loadTasks()
});
</script>
