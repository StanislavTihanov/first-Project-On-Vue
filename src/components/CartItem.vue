<script setup>

    const emit = defineEmits(['onClickRemove'])


    defineProps ({
        id: Number,
        title: String,
        imageUrl: String,
        price: Number
    })
</script>

<template>
    <div  class="flex border items-center border-slate-200 p-4 rounded-xl gap-3 mb-5">
        <img class="w-16 h-16" :src="imageUrl" alt="Sneaker">

        <div class="flex flex-col flex-1">
            <p>{{ title }}</p>
            <div class="flex justify-between mt-2">
                <b>{{ price }}</b>
                <img @click="emit('onClickRemove')" class="opacity-40 hover:opacity-100 cursor-pointer" src="/close.svg" alt="icon close">
            </div>
        </div>
    </div>
</template>