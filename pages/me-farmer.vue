<template>
    <div class="mx-auto my-60 px-4">
        <div>

            <div class="bg-gray-100 relative shadow rounded-lg w-5/6 md:w-5/6 lg:w-4/6 xl:w-3/6 mx-auto">
                <div class="flex ml-10">
                    <img src="https://avatars.githubusercontent.com/u/63314911?v=4" alt=""
                         class="rounded-full mx-auto absolute -top-20 w-32 h-32 shadow-md border-4 border-white transition duration-200 transform hover:scale-110">
                </div>

                <div class="mt-16">
                    <h1 class="font-bold ml-7 text-3xl text-gray-900">{{ userData.firstName }}
                        {{ userData.secondName }}</h1>
                    <p class="ml-7 text-lg text-gray-400 font-medium">Профиль фермера</p>
                    <p>
                        <span>

                        </span>
                    </p>

                    <div class="w-full px-7">
                        <h1 class="text-3xl text-black mt-5">Редактировать настройки</h1>
                        <div class="controls">
                            <div class="grid grid-cols-1 lg:grid-cols-2 gap-4">
                                <div class="control">
                                    <h5 class="mt-2 text-xl text-black">Имя</h5>
                                    <div class="form-control w-full mt-0.5">
                                        <input type="text"
                                               class="input input-bordered border-black bg-transparent w-full text-black"
                                               v-model="fname"/>
                                    </div>
                                </div>
                                <div class="control">
                                    <h5 class="mt-2 text-xl text-black">Фамилия</h5>
                                    <div class="form-control w-full mt-0.5">
                                        <input type="text"
                                               class="input input-bordered border-black bg-transparent w-full text-black"
                                               v-model="sname"/>
                                    </div>
                                </div>

                                <div class="control">
                                    <h5 class="mt-2 text-xl text-black">Реквизиты для зачисления</h5>
                                    <div class="form-control w-full mt-0.5">
                                        <input type="text"
                                               class="input input-bordered border-black bg-transparent w-full text-black"
                                               v-model="cardNumber"
                                        />
                                    </div>
                                </div>

                                <div class="control">
                                    <h5 class="mt-2 text-xl text-black">Банк получатель</h5>
                                    <div class="form-control w-full mt-0.5">
                                        <select type="text"
                                                class="input input-bordered border-black bg-transparent w-full text-black"
                                        >
                                            <option>СберБанк</option>
                                            <option>АльфаБанк</option>
                                            <option>ПочтаБанк</option>
                                            <option>Qiwi</option>
                                        </select>
                                    </div>
                                </div>


                            </div>
                        </div>
                    </div>
                    <div class="my-5 px-6">
                        <a href="#" class="text-gray-200 block rounded-lg text-center font-medium leading-6 px-6 py-3 bg-gray-900 hover:bg-black hover:text-white truncate" @click="updateInfo">Сохранить</a>
                    </div>
                    <div class="my-5 px-6 w-full">
                        <h1 class="text-3xl text-black mt-1">Ваши запланированные поставки</h1>
                        <div class="w-full shadow-lg mt-6">
                            <div class="md:p-8 p-5 dark:bg-gray-900 bg-white rounded-md">
                                <div class="px-4 flex items-center justify-between">
                                    <span  tabindex="0" class="focus:outline-none  text-base font-bold dark:text-gray-100 text-gray-800">Май 2020</span>
                                    <div class="flex items-center">
                                        <button aria-label="calendar backward" class="focus:text-gray-400 hover:text-gray-400 text-gray-800 dark:text-gray-100">
                                            <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-chevron-left" width="24" height="24" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                                                <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                                                <polyline points="15 6 9 12 15 18" />
                                            </svg>
                                        </button>
                                        <button aria-label="calendar forward" class="focus:text-gray-400 hover:text-gray-400 ml-3 text-gray-800 dark:text-gray-100">
                                            <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler  icon-tabler-chevron-right" width="24" height="24" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                                                <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                                                <polyline points="9 6 15 12 9 18" />
                                            </svg>
                                        </button>

                                    </div>
                                </div>
                                <div class="flex items-center justify-between pt-12 overflow-x-auto">
                                    <table class="w-full">
                                        <thead>
                                        <tr>
                                            <th>
                                                <div class="w-full flex justify-center">
                                                    <p class="text-base font-medium text-center text-gray-800 dark:text-gray-100">Пн</p>
                                                </div>
                                            </th>
                                            <th>
                                                <div class="w-full flex justify-center">
                                                    <p class="text-base font-medium text-center text-gray-800 dark:text-gray-100">Вт</p>
                                                </div>
                                            </th>
                                            <th>
                                                <div class="w-full flex justify-center">
                                                    <p class="text-base font-medium text-center text-gray-800 dark:text-gray-100">Ср</p>
                                                </div>
                                            </th>
                                            <th>
                                                <div class="w-full flex justify-center">
                                                    <p class="text-base font-medium text-center text-gray-800 dark:text-gray-100">Чт</p>
                                                </div>
                                            </th>
                                            <th>
                                                <div class="w-full flex justify-center">
                                                    <p class="text-base font-medium text-center text-gray-800 dark:text-gray-100">Пт</p>
                                                </div>
                                            </th>
                                            <th>
                                                <div class="w-full flex justify-center">
                                                    <p class="text-base font-medium text-center text-gray-800 dark:text-gray-100">Сб</p>
                                                </div>
                                            </th>
                                            <th>
                                                <div class="w-full flex justify-center">
                                                    <p class="text-base font-medium text-center text-gray-800 dark:text-gray-100">Вс</p>
                                                </div>
                                            </th>
                                        </tr>
                                        </thead>
                                        <tbody>
                                        <tr>
                                            <td class="pt-6">
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center"></div>
                                            </td>
                                            <td class="pt-6">
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center"></div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center"></div>
                                            </td>
                                            <td class="pt-6">
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">1</p>
                                                </div>
                                            </td>
                                            <td class="pt-6">
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">2</p>
                                                </div>
                                            </td>
                                            <td class="pt-6">
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100">3</p>
                                                </div>
                                            </td>
                                            <td class="pt-6">
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100">4</p>
                                                </div>
                                            </td>
                                        </tr>
                                        <tr>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">5</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">6</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">7</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="w-full h-full">
                                                    <div class="flex items-center justify-center w-full rounded-full cursor-pointer">
                                                        <p class="text-base text-gray-500 dark:text-gray-100 font-medium">8</p>
                                                    </div>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">9</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100">10</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100">11</p>
                                                </div>
                                            </td>
                                        </tr>
                                        <tr>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">12</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">13</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">14</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">15</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">16</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100">17</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100">18</p>
                                                </div>
                                            </td>
                                        </tr>
                                        <tr>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">19</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">20</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">21</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">22</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">23</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100">24</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100">25</p>
                                                </div>
                                            </td>
                                        </tr>
                                        <tr>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">26</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">27</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">28</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">29</p>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="px-2 py-2 cursor-pointer flex w-full justify-center">
                                                    <p class="text-base text-gray-500 dark:text-gray-100 font-medium">30</p>
                                                </div>
                                            </td>
                                        </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="my-5 px-6">
                        <a class="text-gray-200 block rounded-lg text-center font-medium leading-6 px-6 py-3 bg-gray-900 hover:bg-black hover:text-white truncate cursor-pointer" @click="navigateTo('/myproducts')">Запланированные поставки</a>
                    </div>
                    <div class="w-full mt-4">
                        <h3 class="font-medium text-gray-900 text-left px-7">Недавние действия</h3>
                        <div class="w-full flex flex-col items-center overflow-hidden text-sm">
                            <a href="#"
                               class="w-full border-t border-gray-100 text-gray-600 py-4 pl-6 pr-3 w-full block hover:bg-gray-100 transition duration-150">
                                <img src="https://avatars.githubusercontent.com/u/63314911?v=4" alt=""
                                     class="rounded-full h-6 shadow-md inline-block mr-2">
                                Обновил склад своих продуктов
                                <span class="text-gray-500 text-xs">11 min ago</span>
                            </a>

                            <a href="#"
                               class="w-full border-t border-gray-100 text-gray-600 py-4 pl-6 pr-3 w-full block hover:bg-gray-100 transition duration-150 overflow-hidden">
                                <img src="https://avatars.githubusercontent.com/u/63314911?v=4" alt=""
                                     class="rounded-full h-6 shadow-md inline-block mr-2">
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

const fname = ref(null);
const sname = ref(null);
const cardNumber = ref(null);

const userId = useCookie('userId')
if (!userId.value) {
    navigateTo('/login')
}

const userData = ref({});
console.log(userId)
await axios.get('https://hackapi.aspire.su/user?userId=' + userId.value).then(response => {
    console.log(response.data)
    userData.value = response.data;

    fname.value = response.data.firstName;
    sname.value = response.data.secondName;
    cardNumber.value = response.data.card_number;
})


const updateInfo = async () => {
    const config = {
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
        }
    };

    axios.post('https://hackapi.aspire.su/update', qs.stringify({
        userId: userId.value,
        firstName: fname.value,
        secondName: sname.value,
        card_number: cardNumber.value
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
    name: "me-farmer"
}
</script>

<style scoped>

</style>