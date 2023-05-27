<template>
    <div class="container mx-auto px-4">
       <div class="stats-monitor">
           <div class="stats shadow w-full">

               <div class="stat bg-gray-700">
                   <div class="stat-figure text-secondary">
                       <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" class="inline-block w-8 h-8 stroke-current"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                   </div>
                   <div class="stat-title">Входов на сайт</div>
                   <div class="stat-value">31,000</div>
                   <div class="stat-desc">Май 1 - Июль 1</div>
               </div>

               <div class="stat bg-gray-700">
                   <div class="stat-figure text-secondary">
                       <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" class="inline-block w-8 h-8 stroke-current"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6V4m0 2a2 2 0 100 4m0-4a2 2 0 110 4m-6 8a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4m6 6v10m6-2a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4"></path></svg>
                   </div>
                   <div class="stat-title">Пользователей</div>
                   <div class="stat-value">{{adminData.users}}</div>
                   <div class="stat-desc">↗︎ Быстрый рост (150%)</div>
               </div>

               <div class="stat bg-gray-700">
                   <div class="stat-figure text-secondary">
                       <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" class="inline-block w-8 h-8 stroke-current"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6V4m0 2a2 2 0 100 4m0-4a2 2 0 110 4m-6 8a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4m6 6v10m6-2a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4"></path></svg>
                   </div>
                   <div class="stat-title">Фермеров</div>
                   <div class="stat-value">{{adminData.farmers}}</div>
                   <div class="stat-desc">↗︎ Быстрый рост (50%)</div>
               </div>

               <div class="stat bg-gray-700">
                   <div class="stat-figure text-secondary">
                       <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" class="inline-block w-8 h-8 stroke-current"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 8h14M5 8a2 2 0 110-4h14a2 2 0 110 4M5 8v10a2 2 0 002 2h10a2 2 0 002-2V8m-9 4h4"></path></svg>
                   </div>
                   <div class="stat-title">Продуктов на складе</div>
                   <div class="stat-value">{{adminData.products_count}}</div>
                   <div class="stat-desc">↘︎ 90 (14%)</div>
               </div>

           </div>
           <button class="btn mt-3">Экспортировать аналитику</button>

       </div>
    </div>
</template>

<script setup>
import axios from "axios";

const userId = useCookie('userId')
if (!userId.value) {
    navigateTo('/login')
}

const userData = ref({});
const adminData = ref({})

await axios.get('https://hackapi.aspire.su/user?userId=' + userId.value).then(response => {
    console.log(response.data)
    userData.value = response.data;

    axios.get('https://hackapi.aspire.su/admin').then(response => {
        adminData.value = response.data;
    })
})
</script>

<script>
export default {
    name: "admin"
}
</script>

<style scoped>

</style>