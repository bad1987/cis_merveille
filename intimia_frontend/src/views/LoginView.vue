<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const username = ref('')
const password = ref('')
const loading = ref(false)
const error = ref(null)

async function handleSubmit(event) {
    event.preventDefault()
    console.log("submitting the form");
    loading.value = true
    error.value = null
    const post_data = {
        username: username.value,
        password: password.value,
    }

    try {
        const response = await fetch('http://127.0.0.1:8000/api/login/', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json'
            },
            body: JSON.stringify(post_data)
        })

        if (response.ok) {
            const data = await response.json()
            console.log(data)
            // save returned token to localstorage
            localStorage.setItem('token', data.token)
            // redirect to home
            router.push({'name': 'home'})
        }
        else {
            const _error = await response.json()
            console.log(_error);
            if (response.status === 400) {
                error.value = _error.error
            }
        }
        
    }
    catch (error) {
        console.log(error)
    }
    loading.value = false
}
</script>

<template>
    <div class="w-full max-w-md mx-auto min-h-screen">
        <h1 class="text-2xl font-bold mb-6">Login</h1>
        <!-- if erros, display all -->
        <!-- <div v-show="error"> -->
            <div v-if="error"
                class="bg-red-500 text-white w-fit text-sm py-1 px-3 rounded-md mt-2">
                {{ error }}
            </div>
        <!-- </div> -->

        <div v-if="loading"
            class="fixed top-0 left-0 w-full h-full flex justify-center items-center bg-black bg-opacity-50 z-50">
            <div class="animate-spin rounded-full h-32 w-32 border-t-2 border-b-2 border-gray-900"></div>
        </div>

        <form @submit.prevent="handleSubmit" class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4" >
            <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
                    Username
                </label>
                <input
                    v-model="username"
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="username" type="text" placeholder="Username">
            </div>
            <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="password">
                    Password
                </label>
                <input
                    v-model="password"
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"
                    id="password" type="password" placeholder="******************">
            </div>
            <div class="flex items-center justify-between">
                <button
                    type="submit"
                    class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline">
                    Sign In
                </button>
            </div>
        </form>
    </div>
</template>