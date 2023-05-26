<template>
    <div class="mx-auto my-60">
        <div>

            <div class="bg-gray-100 relative shadow rounded-lg w-5/6 md:w-5/6 lg:w-4/6 xl:w-3/6 mx-auto">
                <div class="flex ml-10">
                    <img src="https://daisyui.com/images/stock/photo-1534528741775-53994a69daeb.jpg" alt="" class="rounded-full mx-auto absolute -top-20 w-32 h-32 shadow-md border-4 border-white transition duration-200 transform hover:scale-110">
                </div>

                <div class="mt-16">
                    <h1 class="font-bold ml-7 text-3xl text-gray-900">Покупатель Обыкновенный</h1>
                    <p class="ml-7 text-sm text-gray-400 font-medium">Профиль пользователя</p>
                    <p>
                        <span>

                        </span>
                    </p>
                    <div class="my-5 px-6">
                        <a href="#" class="text-gray-200 block rounded-lg text-center font-medium leading-6 px-6 py-3 bg-gray-900 hover:bg-black hover:text-white truncate">Перейти в профиль фермера</a>
                    </div>

                    <div class="w-full">
                        <h3 class="font-medium text-gray-900 text-left px-6">Недавние действия</h3>
                        <div class="mt-5 w-full flex flex-col items-center overflow-hidden text-sm">
                            <a href="#" class="w-full border-t border-gray-100 text-gray-600 py-4 pl-6 pr-3 w-full block hover:bg-gray-100 transition duration-150">
                                <img src="https://daisyui.com/images/stock/photo-1534528741775-53994a69daeb.jpg" alt="" class="rounded-full h-6 shadow-md inline-block mr-2">
                                Обновил склад своих продуктов
                                <span class="text-gray-500 text-xs">11 min ago</span>
                            </a>

                            <a href="#" class="w-full border-t border-gray-100 text-gray-600 py-4 pl-6 pr-3 w-full block hover:bg-gray-100 transition duration-150 overflow-hidden">
                                <img src="https://daisyui.com/images/stock/photo-1534528741775-53994a69daeb.jpg" alt="" class="rounded-full h-6 shadow-md inline-block mr-2">
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
const userId = useCookie('userId')
if (!userId.value) {
    navigateTo('/login')
}
</script>

<script>
export default {
    name: "me"
}
</script>

<style scoped>

</style>