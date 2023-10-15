<script setup>
import { ref } from 'vue'

const username = ref('')
const email = ref('')
const first_name = ref('')
const last_name = ref('')
const password = ref('')
const password2 = ref('')
const loading = ref(false)
const errors = ref(null)

async function handleSubmit(event) {
    event.preventDefault()
    console.log("submitting the form");
    loading.value = true
    errors.value = null
    const post_data = {
        username: username.value,
        email: email.value,
        first_name: first_name.value,
        last_name: last_name.value,
        password: password.value,
        password2: password2.value
    }

    try {
        const response = await fetch('http://127.0.0.1:8000/api/register/', {
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
        }
        else {
            const error = await response.json()
            console.log(error);
            if (response.status === 400) {
                errors.value = error
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
        <h1 class="text-2xl font-bold mb-6">Register</h1>
        <!-- if erros, display all -->
        <div v-if="errors">
            <div v-for="field in Object.keys(errors)" :key="field"
                class="bg-red-500 text-white w-fit text-sm py-1 px-3 rounded-md mt-2">
                {{ field }}: {{ errors[field].join(', ') }}
            </div>
        </div>

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
                <label class="block text-gray-700 text-sm font-bold mb-2" for="email">
                    Email
                </label>
                <input
                    v-model="email"
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="email" type="email" placeholder="Email">
            </div>
            <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="first_name">
                    First Name
                </label>
                <input
                    v-model="first_name"
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="first_name" type="text" placeholder="First Name">
            </div>
            <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="last_name">
                    Last Name
                </label>
                <input
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="last_name" type="text" placeholder="Last Name">
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
            <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="password2">
                    Confirm Password
                </label>
                <input
                    v-model="password2"
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"
                    id="password2" type="password" placeholder="******************">
            </div>
            <div class="flex items-center justify-between">
                <button
                    type="submit"
                    class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline">
                    Sign Up
                </button>
            </div>
        </form>
    </div>
</template>