<script setup>
import { ref, onMounted } from 'vue';

const user = ref(null);

onMounted(async () => {
  try {
    // get the token from localstorage
    const token = localStorage.getItem('token');
    console.log(token);
    // get the logged in user
    const response = await fetch('http://127.0.0.1:8000/api/user/', {
      headers: {
        Authorization: `Token ${token}`,
        'Accept': 'application/json'
      }
    })
    if (response.ok) {
      const data = await response.json();
      user.value = data
      console.log(data);
    }
    else{
      const error = await response.json()
      console.log(error);
    }
  } catch (error) {
    console.log(error)
    
  }
})
</script>

<template>
  <main>
    <div class="bg-blue-500 text-white p-4">
      <h1 class="text-xl font-bold">Welcome to intimia</h1>
    </div>
  </main>
</template>
