<template>
    <div class="min-h-screen hero bg-base-300">
      <div class="flex flex-col items-center justify-center text-center hero-content sm:flex-row">
        <div class="max-w-md px-4">
            <h1 class="text-4xl font-extrabold">
                <span id="typewriter"></span>Malwares.
                <span class="block">Get Money.</span>
            </h1>
            
            <p class="py-6 text-lg sm:text-xl">
                Best marketplace to trade malwares. We provide a secure platform to buy and sell malwares.
            </p>
            <router-link v-if="isAuthenticated" to="/search" class="btn btn-primary">Go to Products</router-link>
            <router-link v-else to="/register" class="btn btn-primary">Get Started</router-link>
        </div>
      </div>
    </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import Typed from 'typed.js';
import axios from 'axios';
import { useRouter } from 'vue-router';

export default {
    name: 'Index',
    setup() {
        const isAuthenticated = ref(false);
        const router = useRouter();

        const checkAuth = async () => {
            if (isAuthenticated.value) return;
            
            try {
                const response = await axios.get('/user-details')
                isAuthenticated.value = response.data.authenticated
            } catch (error) {
                isAuthenticated.value = false
            }
        };

        onMounted(() => {
            const options = {
                strings: ["Buy", "Sell"],
                typeSpeed: 250,
                backSpeed: 150,
                loop: true,
            };

            new Typed('#typewriter', options);

            checkAuth();
        });

        window.addEventListener('login', () => {
            checkAuth();
        });

        window.addEventListener('logout', () => {
            isAuthenticated.value = false;
        });

        return {
            isAuthenticated
        };
    }
}
</script>