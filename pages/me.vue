<template>
    <div class="mx-auto my-60 px-4">
        <div>

            <div class="bg-gray-100 relative shadow rounded-lg w-5/6 md:w-5/6 lg:w-4/6 xl:w-3/6 mx-auto">
                <div class="flex ml-10">
                    <img src="https://avatars.githubusercontent.com/u/63314911?v=4" alt="" class="rounded-full mx-auto absolute -top-20 w-32 h-32 shadow-md border-4 border-white transition duration-200 transform hover:scale-110">
                </div>

                <div class="mt-16">
                    <h1 class="font-bold ml-7 text-3xl text-gray-900">{{userData.firstName}} {{userData.secondName}}</h1>
                    <p class="ml-7 text-sm text-gray-400 font-medium">Профиль пользователя</p>
                    <p>
                        <span>

                        </span>
                    </p>
                    <div class="my-5 px-6">
                        <a href="#" class="text-gray-200 block rounded-lg text-center font-medium leading-6 px-6 py-3 bg-gray-900 hover:bg-black hover:text-white truncate" @click="makeMeSeller">{{(userData.role === "SELLER" || userData.role === "ADMIN") ? 'Перейти в профиль фермера' : 'Создать профиль фермера'}}</a>
                    </div>

                    <div class="w-full">
                        <h3 class="font-medium text-gray-900 text-left px-6">Недавние действия</h3>
                        <div class="mt-5 w-full flex flex-col items-center overflow-hidden text-sm">
                            <a href="#" class="w-full border-t border-gray-100 text-gray-600 py-4 pl-6 pr-3 w-full block hover:bg-gray-100 transition duration-150">
                                <img src="https://avatars.githubusercontent.com/u/63314911?v=4" alt="" class="rounded-full h-6 shadow-md inline-block mr-2">
                                Обновил склад своих продуктов
                                <span class="text-gray-500 text-xs">11 min ago</span>
                            </a>

                            <a href="#" class="w-full border-t border-gray-100 text-gray-600 py-4 pl-6 pr-3 w-full block hover:bg-gray-100 transition duration-150 overflow-hidden">
                                <img src="https://avatars.githubusercontent.com/u/63314911?v=4" alt="" class="rounded-full h-6 shadow-md inline-block mr-2">
                                Зарегистрировался на платформе
                                <span class="text-gray-500 text-xs">5 days ago</span>
                            </a>

                        </div>
                    </div>
                </div>
            </div>

        </div>
    </div>
</template>

<script setup>
import axios from "axios";
import qs from "qs";

const userId = useCookie('userId')
if (!userId.value) {
    navigateTo('/login')
}

const userData = ref({});
console.log(userId)
await axios.get('https://hackapi.aspire.su/user?userId=' + userId.value).then(response => {
    console.log(response.data)
    userData.value = response.data;
})

const makeMeSeller = async () => {
    if (userData.value.role === "SELLER" || userData.value.role === "ADMIN") return window.location.href = '/me-farmer';
    const config = {
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
        }
    };

    axios.post('https://hackapi.aspire.su/role', qs.stringify({
        userId: userId.value,
        role: 'SELLER'
    }), config)
        .then(response => {
            console.log(response.data.message)
            if(response.data.message) {
                error_message.value = response.data.message
            } else {

                window.location.reload();
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
    name: "me"
}
</script>

<style scoped>

</style>