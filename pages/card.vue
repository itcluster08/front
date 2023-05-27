<template>
    <div class="container mx-auto px-4">
        <h1 class="text-4xl text-white font-bold">Ваша корзина</h1>
        <div class="loading" v-if="loading">
            <h1>ЗАГРУЗКА</h1>
            <progress class="progress progress-info w-full"></progress>

        </div>
        <div v-else>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-y-5 lg:gap-y-20 mt-10 justify-items-center lg:justify-items-start" v-if="shopCard.length">
                <div class="card card-compact w-96 bg-base-100 shadow-md bg-gray-700" v-for="(item, index) in shopCard">
                    <figure><img :src="products[item.id].image" class="w-full"/></figure>
                    <div class="card-body">
                        <div class="flex justify-between items-center">
                            <div class="card-title text-white flex items-center truncate">
                                <div>
                                    <h1 class="text-3xl">{{ products[item.id].name }}</h1>
                                    <div class="rating rating-md">
                                        <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400"/>
                                        <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400"
                                               checked/>
                                        <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400"/>
                                        <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400"/>
                                        <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400"/>
                                    </div>
                                </div>

                            </div>
                        </div>

                        <div class="card-actions justify-end flex justify-between items-center mt-1">
                            <div class="text-xl text-white font-bold">
                                <span class="mr-1">{{ calculatePrice(item.price, item.quantity)[0] }}₽</span>
                                <small class="text-gray-300 text-sm">за {{ item.quantity }} штук <span
                                    class="text-green-500">{{ calculatePrice(item.price, item.quantity)[1] > 1 ? `- скидка ${Math.round(calculatePrice(item.price, item.quantity)[1])}₽` : '' }}</span></small>
                            </div>
                            <button class="btn btn-sm btn-primary" @click="removeFromShopper(index)">Убрать</button>
                        </div>
                    </div>
                </div>
            </div>

            <div v-if="!shopCard.length">
                <h1 class="text-xl">Ой! Походу тут пусто! Как насчет набрать продуктов?</h1>
                <button class="btn btn-primary btn-wide mt-3" @click="navigateTo('/')">Перейти в каталог</button>
            </div>

            <button class="btn btn-primary btn-wide mt-3" @click="navigateTo('/buy')" v-else>
                Оплатить корзину
            </button>
        </div>
    </div>
</template>


<script setup>
import axios from "axios";

const userId = useCookie('userId')
if (!userId.value) {
    navigateTo('/login')
}

const loading = ref(true);
const products = ref([]);
const shopCard = ref([]);


await axios.get('https://hackapi.aspire.su/products').then(response => {
    products.value = response.data;
})

function calculatePrice(price, quantity) {
    const discountThreshold = 10; // Define the threshold for applying the discount
    const discountPercentage = 0.1; // Define the discount percentage (10% in this example)
    let totalPrice = price * quantity;
    let totalDiscount = 0;

    if (quantity >= discountThreshold) {
        const discountAmount = totalPrice * discountPercentage;
        totalPrice -= discountAmount;
        totalDiscount += discountAmount;
    }

    return [totalPrice, totalDiscount];
}

function removeFromShopper(index) {
    const card = JSON.parse(localStorage.getItem('shopItems')) || [];
    card.splice(index, 1)
    localStorage.setItem('shopItems', JSON.stringify(card))
}

onMounted(() => {
    setInterval(() => {
        shopCard.value = JSON.parse(localStorage.getItem('shopItems')) || [];
        loading.value = false;
    }, 200)
})


</script>

<script>
export default {
    name: "card",
}
</script>

<style scoped>
</style>