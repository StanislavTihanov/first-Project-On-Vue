<script setup> 

    defineProps({
        totalPrice: Number
    })

    const emit = defineEmits(['openDrawer']);
</script>

<template>
<header class="flex justify-between border-b border-slate-300 px-5 py-5 flex-wrap gap-x-4 gap-y-5 header">
    <router-link to="/">
        <div class="flex items-center gap-4">
            <img class="w-10" src="/logo.png" alt="logo"/>
            <div>
                <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
                <p class="text-slate-400">Магазин лчших кросовок</p>
            </div>
        </div>
    </router-link>
    <ul class="flex items-center gap-5 flex-wrap">
        <li @click="() => emit('openDrawer')" class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
            <img src="/cart.svg" alt="cart">
            <b>{{ totalPrice }} p</b>
        </li>
        <router-link to="/Favorites">
            <li class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
                <img src="/heart.svg" alt="heart">
                <span>Закладки</span>
            </li>
        </router-link>
        <li class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
            <img src="/profile.svg" alt="cart">
            <span>Профиль</span>
        </li>
    </ul>
    </header>
</template>