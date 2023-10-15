<script setup>
import { onBeforeMount } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

onBeforeMount(async() => {
    // get the token from localstorage
    const token = localStorage.getItem('token')
    // log out the user
    const response = await fetch('http://127.0.0.1:8000/api/logout/', {
        method: 'POST',
        headers: {
            Authorization: `Token ${token}`,
            'Accept': 'application/json'
        }
    })
    if (response.ok) {
        const data = await response.json()
        console.log(data)
        // delete the token from localstorage
        // localStorage.removeItem('token')
        // redirect to login page
        router.push({ name: 'login' })
    }
})
</script>