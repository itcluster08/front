<template>
    <div class="container mx-auto px-4">
        <h1 class="text-4xl text-white font-bold">Ваши отправленные продукты</h1>
        <div class="loading" v-if="loading">
            <h1>ЗАГРУЗКА</h1>
            <progress class="progress progress-info w-full"></progress>

        </div>
        <div v-else>
            <div
                class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-y-5 lg:gap-y-20 mt-10 justify-items-center lg:justify-items-start">
                <div class="card card-compact w-96 bg-base-100 shadow-md bg-gray-700" v-for="(item, index) in products">
                    <figure><img :src="item.Food?.image" class="w-full"/></figure>
                    <div class="card-body">
                        <div class="flex justify-between items-center">
                            <div class="card-title text-white flex items-center truncate">
                                <div>
                                    <h1 class="text-3xl">{{ item.Food.name }}</h1>
                                    <p class="text-gray-200 text-md">Приедут на склад
                                        {{ new Date(item.destinationTime).toLocaleDateString() }}</p>
                                </div>

                            </div>
                        </div>

                        <div class="card-actions justify-end flex justify-between items-center mt-1">
                            <div class="text-xl text-white font-bold    ">
                                <span class="mr-1">{{ item.count }} шт</span>
                                <small class="text-gray-300 text-sm">приблизительная выручка: <span
                                    class="text-white text-lg">{{ item.count * item.Food.price }}₽</span></small>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="card card-compact w-96 bg-base-100 shadow-md bg-gray-700">
                    <div class="card-body grid place-content-center">

                        <label for="my-modal" class="btn btn-lg btn-circle">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 rotate-45" fill="none"
                                 viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                      d="M6 18L18 6M6 6l12 12"/>
                            </svg>
                        </label>
                    </div>
                </div>
            </div>
        </div>

        <h1 class="text-4xl text-white font-bold mt-4">Статистика</h1>
        <div class="stats shadow mt-4 w-full">

            <div class="stat bg-gray-700">
                <div class="stat-figure text-secondary">
                    <h1 class="text-4xl font-bold">?</h1>
                </div>
                <div class="stat-title">Поставлено товаров</div>
                <div class="stat-value text-secondary">{{products.reduce((a, b) => a + b.count, 0)}}</div>
                <div class="stat-desc">Недостаточно данных для аналитики</div>
            </div>

            <div class="stat bg-gray-700">
                <div class="stat-figure text-green-600">
                    <h1 class="text-4xl font-bold">!</h1>
                </div>
                <div class="stat-title">Отправок</div>
                <div class="stat-value text-green-600">{{products.length}}</div>
                <div class="stat-desc">Недостаточно данных для аналитики</div>
            </div>

            <div class="stat bg-gray-700">
                <div class="stat-figure text-primary">
                    <h1 class="text-4xl font-bold">₽</h1>
                </div>
                <div class="stat-title">Полученная выручка</div>
                <div class="stat-value text-primary">{{products.reduce((a, b) => a + (productsList[b.id]?.price || 0) * b.count, 0)}}₽</div>
                <div class="stat-desc">21% больше чем за прошлый месяц</div>
            </div>

            <div class="stat bg-gray-700">
                <div class="stat-figure text-secondary">
                </div>
                <div class="stat-value text-warning">5.0 ★</div>
                <div class="stat-title">Общий рейтинг</div>
                <div class="stat-desc text-secondary">Всего 32 покупки</div>
            </div>

        </div>
        <input type="checkbox" id="my-modal" class="modal-toggle"/>
        <div class="modal">
            <div class="modal-box bg-white">
                <h3 class="font-bold text-lg text-black">Вы добавляете новый товар!</h3>
                <p class="py-4 text-black">Выберите товар и его количество для отправки.</p>

                <img :src="productsList[productToSend - 1]?.image" alt="">
                <div class="form-control w-full mt-2">
                    <select class="input input-bordered w-full bg-transparent text-black" v-model="productToSend">
                        <option v-for="item in productsList" :value="item.id">{{item.name}}</option>
                    </select>
                    <label class="label">
                        <span class="label-text-alt text-black">Продукт</span>
                    </label>
                </div>
                <div class="form-control w-full mt-2">
                    <input type="number" class="input input-bordered w-full bg-transparent text-black" v-model="countToSend" />
                </div>
                <label class="label">
                    <span class="label-text-alt text-black">Количество товара</span>
                </label>

                <div class="form-control w-full mt-2">
                    <select class="input input-bordered w-full bg-transparent text-black" v-model="selectedSender">
                        <option value="walk">Пешком</option>
                        <option value="car">Автомобиль</option>
                        <option value="auto">Грузовик</option>
                    </select>
                </div>
                <label class="label">
                    <span class="label-text-alt text-black">Метод доставки</span>
                </label>
                <p class="text-black mt-2 font-bold" v-if="canBeSent">Доставка обойдется в {{4 * prices[selectedSender] * countToSend}}₽</p>
                <p class="text-black mt-2 font-bold" v-else>Перегруз! Выберите более вместительное средство.</p>

                <div class="modal-action flex">
                    <label @click="addProduct" :for="canBeSent ? 'my-modal' : 'disabled'" class="btn grow" :class="{'btn-primary': canBeSent, 'btn-error text-white': !canBeSent}">Добавить</label>
                    <label for="my-modal" class="btn text-white">Отмена</label>
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

const loading = ref(true);
const products = ref([]);
const productsList = ref([]);
const productToSend = ref(1);
const countToSend = ref(10);
const selectedSender = ref('car');
const prices = {
    'walk': 1,
    'car': 3,
    'auto': 4
}

const canBeSent = computed(() => {
    if (selectedSender.value === 'walk' && countToSend.value > 10) return false;
    if (selectedSender.value === 'car' && countToSend.value > 100) return false;
    if (selectedSender.value === 'auto' && countToSend.value > 10000) return false;
    return true;
})

const addProduct = () => {
    if (!canBeSent.value) return;
    const config = {
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
        }
    };

    axios.post('https://hackapi.aspire.su/marketplace', qs.stringify({
        farmerId: userId.value,
        foodId: productToSend.value,
        count: countToSend.value
    }), config).then(r => {
        window.location.reload()
    })
}

await axios.get('https://hackapi.aspire.su/farmerproducts?farmerId=' + userId.value).then(response => {
    products.value = response.data;
    loading.value = false;
    axios.get('https://hackapi.aspire.su/products').then(response => {
        productsList.value = response.data;
    })
})

</script>

<script>
export default {
    name: "myproducts",
}
</script>

<style scoped>
</style>