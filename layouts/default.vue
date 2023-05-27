<template>
    <div class="dark">
        <div class="navbar bg-base-100">
            <div class="navbar-start">
                <div class="dropdown">
                    <label tabindex="0" class="btn btn-ghost btn-circle">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24"
                             stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                  d="M4 6h16M4 12h16M4 18h7"/>
                        </svg>
                    </label>
                    <ul tabindex="0"
                        class="menu menu-compact dropdown-content mt-3 p-2 shadow bg-base-100 rounded-box w-52">
                        <li><a @click="navigateTo('/')">Главная</a></li>
                        <li><a @click="navigateTo('/helper')">Справка от администраторов</a></li>
                        <li><a @click="navigateTo('/about')">О нас</a></li>
                    </ul>
                </div>
            </div>
            <div class="navbar-center">
                <a class="btn btn-ghost normal-case text-xl" @click="navigateTo('/')">ДобрыйЧеченец</a>
            </div>
            <div class="navbar-end" v-if="userId">
                <div class="dropdown dropdown-end">
                    <label tabindex="0" class="btn btn-ghost btn-circle hidden md:flex">
                        <div class="indicator">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24"
                                 stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                      d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"/>
                            </svg>
                            <span class="badge badge-sm indicator-item"
                                  :class="{'badge-error': card.length > 0}">{{ card.length }}</span>
                        </div>
                    </label>
                    <div tabindex="0" class="mt-3 card card-compact dropdown-content w-52 bg-base-100 shadow">
                        <div class="card-body">
                            <span class="font-bold text-lg">{{ card.length }} товаров</span>
                            <span
                                class="text-info">Итого: {{ card.length ? card.reduce((a, b) => a + b.price * b.quantity || 0, 0) : 0 }}₽</span>
                            <div class="card-actions">
                                <button class="btn btn-primary btn-block" @click="navigateTo('/card')">В корзину
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="dropdown dropdown-end">
                    <label tabindex="0" class="btn btn-ghost btn-circle avatar">
                        <div class="w-10 rounded-full">
                            <img src="https://avatars.githubusercontent.com/u/63314911?v=4"/>
                        </div>
                    </label>
                    <ul tabindex="0"
                        class="menu menu-compact dropdown-content mt-3 p-2 shadow bg-base-100 rounded-box w-52">
                        <li><a @click="navigateTo('/me')">Профиль</a></li>
                        <li v-if="userData.role === 'SELLER'"><a @click="navigateTo('/me-farmer')">Профиль фермера</a></li>
                        <li v-if="userData.role === 'ADMIN'"><a @click="navigateTo('/admin')">Профиль администратора</a></li>
                        <li><a>Настройки</a></li>
                        <li><a @click="logOut">Выйти</a></li>
                    </ul>
                </div>
            </div>
            <div class="navbar-end" v-else>
                <button class="btn btn-sm btn-primary" @click="navigateTo('/login')">ВОЙТИ</button>
            </div>
        </div>
        <slot>
        </slot>

        <div class="mb-40"></div>
        <div class="btm-nav md:hidden">
            <button :class="{'active text-success': $route.path === '/' }" @click="navigateTo('/')">
                <label tabindex="0" class="btn btn-ghost btn-circle">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" /></svg>
                </label>
            </button>
            <button :class="{'active text-success': $route.path === '/card' }" @click="navigateTo('/card')">
                <label tabindex="0" class="btn btn-ghost btn-circle">
                    <div class="indicator">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24"
                             stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                  d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"/>
                        </svg>
                        <span class="badge badge-sm indicator-item"
                              :class="{'badge-error': card.length > 0}">{{ card.length }}</span>
                    </div>
                </label>
            </button>
        </div>
    </div>
</template>

<script setup>

import axios from "axios";

const userId = useCookie('userId')

const card = ref([]);
const userData = ref({});

await axios.get('https://hackapi.aspire.su/user?userId=' + userId.value).then(response => {
    userData.value = response.data;
})

onMounted(() => {
    setInterval(() => {
        card.value = JSON.parse(localStorage.getItem('shopItems')) || [];
    }, 200)
})

function logOut() {
    userId.value = null;
    window.location.reload()
}

function calculatePrice(price, quantity) {
    const discountThreshold = 10; // Define the threshold for applying the discount
    const discountPercentage = 0.1; // Define the discount percentage (10% in this example)
    let totalPrice = price * quantity;

    if (quantity >= discountThreshold) {
        const discountAmount = totalPrice * discountPercentage;
        totalPrice -= discountAmount;
    }

    return totalPrice;
}
</script>

<script>
export default {
    name: "default"
}
</script>

<style scoped>

</style>