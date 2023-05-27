<template>
    <div class="container px-4">
        <div class="login-container flex flex-col items-center mt-0 md:mt-60">
            <div class="wrap">
                <h1 class="text-3xl header-font font-semibold text-white">Для продолжения потребуется
                    авторизироваться</h1>

                <div class="more" v-if="registering">
                    <h5 class="mt-2 font-semibold text-xl header-font text-white">Имя</h5>
                    <div class="form-control w-full mt-2">
                        <input type="text" placeholder="Артем" class="input input-bordered w-full"
                               v-model="fname"/>
                    </div>
                    <h5 class="mt-2 font-semibold text-xl header-font text-white">Фамилия</h5>
                    <div class="form-control w-full mt-2">
                        <input type="text" placeholder="Лапкин" class="input input-bordered w-full"
                               v-model="sname"/>
                    </div>
                </div>
                <h5 class="mt-3 font-semibold text-xl header-font text-white">Почта</h5>
                <div class="form-control w-full mt-2">
                    <input type="email" placeholder="saphire.pi@mail.ru" class="input input-bordered w-full"
                           v-model="username"/>
                </div>

                <h5 class="mt-4 font-semibold text-xl header-font text-white">Пароль</h5>
                <div class="form-control w-full mt-2">
                    <input type="password" class="input input-bordered w-full" v-model="password"/>
                </div>

                <div class="flex" :class="{'flex-col': !registering, 'flex-col-reverse': registering}">
                    <button class="btn mt-3 header-font px-12 w-full" @click="auth" :class="{'btn-primary': !registering, 'btn-sm': registering}">Войти</button>
                    <button class="btn mt-3 header-font px-12 w-full" @click="register" :class="{'btn-primary': registering, 'btn-sm': !registering}">Регистрация</button>
                </div>
                <div class="alert alert-error shadow-lg mt-3" v-if="error_message">
                    <div>
                        <svg xmlns="http://www.w3.org/2000/svg" class="stroke-current flex-shrink-0 h-6 w-6" fill="none" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
                        <span>{{error_message}}</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<script setup>

import axios from "axios";
import qs from 'qs';

const userId = useCookie('userId')
if (userId.value) {
    navigateTo('/')
}

const fname = ref(null);
const sname = ref(null);
const password = ref(null);
const username = ref(null);
const registering = ref(false)
let error_message = ref(null);

const auth = async () => {
    if (registering.value) return registering.value = false;
    const config = {
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
        }
    };

    axios.post('https://hackapi.aspire.su/login', qs.stringify({
        password: password.value,
        username: username.value,
    }), config)
        .then(response => {
            console.log(response.data.message)
            if(response.data.message) {
                error_message.value = response.data.message
            } else {
                const userId = useCookie('userId');
                userId.value = response.data.id;

                window.location.href = '/'
            }
        })
        .catch(error => {
            console.error('Error:', error);
        });
}


const register = async () => {
    if (!registering.value) return registering.value = true;
    const config = {
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
        }
    };

    axios.post('https://hackapi.aspire.su/register', qs.stringify({
        password: password.value,
        username: username.value,
        firstName: fname.value,
        secondName: sname.value
    }), config)
        .then(response => {
            console.log(response.data.message)
            if(response.data.message) {
                error_message.value = response.data.message
            } else {
                const userId = useCookie('userId');
                userId.value = response.data.id;

                window.location.href = '/'
            }
        })
        .catch(error => {
            console.log(error)
            if(error.response.data.message) {
                error_message.value = error.response.data.message
            } else
            console.error('Error:', error);
        });
}
</script>

<script>
export default {
    name: "login"
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Nunito:ital,wght@0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;0,1000;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900;1,1000&family=Rubik:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

body {
    font-family: Nunito;
}

.header-font {
    font-family: Rubik !important;
}

.container {
    max-width: 1400px;
    margin-left: auto;
    margin-right: auto;
    padding: 0 1em;
}
</style>