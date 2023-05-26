<template>
    <div class="container mx-auto">
        <h1 class="text-4xl text-white font-bold">Оплата корзины</h1>
        <div class="loading" v-if="loading">
            <h1>ЗАГРУЗКА</h1>
            <progress class="progress progress-info w-full"></progress>

        </div>
        <div v-else>
            <h1 class="text-white text-xl">В вашей корзине {{shopCard.length}} продуктов на сумму {{shopCard.reduce((a, b) => a + calculatePrice(b.price, b.quantity)[0], 0)}}₽</h1>
            <div class="mt-12">
                <h1 class="text-white text-4xl font-bold">Выберите способ оплаты</h1>
                <div class="grid grid-cols-4 gap-4 mt-4">
                    <div class="flex flex-col bg-white border shadow-sm hover:shadow-2xl transition cursor-pointer rounded-xl border-4" v-for="(provider, index) in providers" :class="{'border-info': index === selectedProvider, 'border-white': index !== selectedProvider}" @click="selectedProvider = index">
                        <div class="p-4 flex items-center justify-between">
                            <h3 class="text-xl font-bold text-gray-800">
                                {{provider.name}}
                            </h3>
                            <img :src="provider.img" width="70" height="38" style="height: 38px; width: 70px" alt="">
                        </div>
                    </div>
                </div>
                <ul class="steps steps-vertical lg:steps-horizontal w-full mt-5">
                    <li class="step step-primary">Наберите корзину</li>
                    <li class="step step-primary">Выберите метод оплаты</li>
                    <li class="step" :class="{'step-primary': selectedProvider}">Оплатите покупку</li>
                    <li class="step" :class="{'step-primary': confirm}">Дождитесь заказа</li>
                </ul>

                <div class="form" v-if="selectedProvider && !confirm">
                    <div class="grid grid-cols-3 gap-10 mt-4">
                        <div class="form-control w-full">
                            <h1 class="text-xl text-white">Номер карты</h1>
                            <input type="text" class="input input-bordered w-full mt-2" />
                        </div>

                        <div class="form-control w-full">
                            <h1 class="text-xl text-white">Имя на карте</h1>
                            <input type="text" name="name" class="input input-bordered w-full mt-2" />
                        </div>

                        <div class="form-control w-full">
                            <h1 class="text-xl text-white">CVV КОД</h1>
                            <input type="password" name="cvv" class="input input-bordered w-full mt-2" />
                        </div>

                        <div class="form-control w-full">
                            <h1 class="text-xl text-white">Когда истекает</h1>
                            <input type="month" name="expire" class="input input-bordered w-full mt-2" />
                        </div>

                        <div class="form-control w-full">
                            <h1 class="text-xl text-white">Город</h1>
                            <input type="text" name="expire" class="input input-bordered w-full mt-2" />
                        </div>

                        <div class="form-control w-full">
                            <h1 class="text-xl text-white">Дом</h1>
                            <input type="text" name="expire" class="input input-bordered w-full mt-2" />
                        </div>

                        <div class="form-control w-full">
                            <h1 class="text-xl text-white">Квартира</h1>
                            <input type="text" name="expire" class="input input-bordered w-full mt-2" />
                        </div>
                    </div>

                    <button class="btn btn-wide btn-primary mt-5" @click="doneBuy">Оплатить</button>
                </div>

                <div v-if="confirm" class="mt-10">
                    <h1 class="text-white text-4xl">Заказ оплачен!</h1>
                    <p class="text-xl">Поставьте оценку данному продавцу</p>
                    <div class="rating rating-lg mt-1">
                        <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400" />
                        <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400" />
                        <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400" />
                        <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400" />
                        <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400" />
                    </div>
                </div>
            </div>
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
const confirm = ref(false);
const products = ref([]);
const shopCard = ref([]);

const selectedProvider = ref(null);
const providers = ref([{
    name: 'Visa / MasterCard',
    img: 'https://i.minerea.su/k1Wu'
}, {
    name: 'СберБанк',
    img: 'https://i.minerea.su/ksA2'
}, {
    name: 'MasterCard',
    img: 'https://i.minerea.su/h67D'
}, {
    name: 'Qiwi',
    img: 'https://i.minerea.su/s5cI'
}])


await axios.get('http://localhost:5400/products').then(response => {
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

function doneBuy() {
    confirm.value = true;
    localStorage.setItem('shopItems', '[]')
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
    name: "buy",
}
</script>

<style scoped>
</style>