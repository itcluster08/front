<template>
    <div class="container mx-auto">
        <h1 class="text-4xl">Магазин продуктов</h1>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-y-20 mt-10">
            <div class="card card-compact w-96 bg-base-100 shadow-xl bg-gray-700" v-for="(product, index) in products">
                <figure><img :src="product.image" class="w-full"/></figure>
                <div class="card-body">
                    <div class="flex justify-between items-center">
                        <div class="card-title text-white flex items-center truncate">
                            <div>
                                <h1 class="text-3xl">{{ product.name }}</h1>
                                <div class="rating rating-md">
                                    <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400" />
                                    <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400" checked />
                                    <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400" />
                                    <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400" />
                                    <input type="radio" name="rating-7" class="mask mask-star-2 bg-orange-400" />
                                </div>
                            </div>

                        </div>
                    </div>
                    <h5>Количество товара</h5>
                    <input type="range" min="0" max="100" class="range range-xs" v-model="shopper_range[index]" />

                    <div class="card-actions justify-end flex justify-between items-center mt-1">
                        <div class="text-xl text-white font-bold">
                            <span class="mr-1">{{ calculatePrice(product.price, shopper_range[index])[0] }}₽</span>
                            <small class="text-gray-300 text-sm">{{ product.countInStock }} на складе</small>
                        </div>
                        <button class="btn btn-sm btn-primary" @click="addToShopper(product.id, shopper_range[index], product.price)">В корзину ({{shopper_range[index]}})</button>
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

const products = ref([]);
const shopper_range = ref([]);
for (let i = 0; i < 100; i++) shopper_range.value[i] = 1;

await axios.get('https://hackapi.aspire.su/products').then(response => {
    console.log(response.data)
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

function addToShopper(id, quantity, price) {
   const card = JSON.parse(localStorage.getItem('shopItems')) || [];
   card.push({id, quantity, price})
   localStorage.setItem('shopItems', JSON.stringify(card))
}


</script>

<script>
export default {
    name: "index",
}
</script>

<style scoped>
</style>