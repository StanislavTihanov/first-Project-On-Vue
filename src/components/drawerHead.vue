<script setup>
    import { inject } from 'vue';
    const { closeDrawer } = inject('cart')

</script>

<template>
        <div class="flex items-center gap-5 mb-5">
            <svg @click="closeDrawer" 
            class="opacity-30 cursor-pointer hover:opacity-100 transition hover:-translate-x-1" xmlns="http://www.w3.org/2000/svg" width="20" height="15" viewBox="0 0 73 52" fill="none">
                <path d="M1.52079 23.5902C0.15637 24.9595 0.160273 27.1755 1.52951 28.54L23.8426 50.7745C25.2118 52.139 27.4279 52.1351 28.7923 50.7658C30.1567 49.3966 30.1528 47.1805 28.7836 45.8161L8.94976 26.052L28.7138 6.21817C30.0783 4.84893 30.0744 2.63285 28.7051 1.26843C27.3359 -0.0959954 25.1198 -0.0920918 23.7554 1.27715L1.52079 23.5902ZM72.9938 22.4392L3.99386 22.5607L4.00619 29.5607L73.0061 29.4392L72.9938 22.4392Z" fill="black"/>
            </svg>
            <h2 class="text-2xl font-bold">Корзина</h2>
        </div>
</template>