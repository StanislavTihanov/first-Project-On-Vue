<script setup>


    defineProps({
        id: Number,
        title: String,
        imageUrl: String,
        price: Number,
        isFavorite: Boolean,
        isAdded: Boolean,
        onClickFavorite: Function,
        onClickAdd: Function
    });


</script>

<template>
        <div class="relative border bg-white border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl">
            <img class="absolute top-8 left-8" @click="onClickFavorite" :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'" alt="like">
            <img :src="imageUrl" alt="Sneaker">
            <p class="mt-5">{{ title }}</p>
            <div class="flex justify-between mt-5">
                <div class="flex flex-col">
                <span class="text-slate-400">Цена:</span>
                <b>{{price}} руб</b>
                </div>
                <img @click="onClickAdd" :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="button plus">
            </div>
        </div>
</template>