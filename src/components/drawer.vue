<script setup>
    import drawerHead from './drawerHead.vue'
    import CartItemList from './CartItemList.vue'
    import infoBlock from './InfoBlock.vue'
    import axios from 'axios';
    import {  ref, computed, inject } from 'vue'


    const props = defineProps ({
        totalPrice: Number,
        vatPrice: Number,
        cartButtonDisabled: Boolean,
    })
    
    const {  cart, closeDrawer} = inject('cart')

    const isCreating = ref(false)

    const createOrder = async () => {
    try {
        isCreating.value = true
        const {data} = await axios.post(`https://8dd1032de6161988.mokky.dev/orders`, {
            items: cart.value,
            totalPrice: props.totalPrice.value,
        });

        cart.value = []
        
        return data;
    } catch (err) {
        console.log(err)
    } finally {
        isCreating.value = false
    }
}
const cartIsEmpty = computed(() => cart.value.length === 0)
const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)
</script>


<template>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white h-full  w-96 fixed right-0 top-0 z-20 p-8 flex flex-col justify-between overflow-auto">

        <drawerHead/>
        

        <infoBlock 
            title="Заказ оформлен"
            description="Ваш заказ № скоро будет передан курьерской доставке"
            image-url="/order-success-icon.png"
        />


        <div v-if="!totalPrice">
        <infoBlock 
            title="Корзина пуста"
            description="Добавь хотя бы один товар, чтобы сделать заказ"
            image-url="/package-icon.png"
        />
        </div>
        <CartItemList/>

        <div v-if="totalPrice" class="flex flex-col gap-4 mb-6">
            <div class="flex gap-1">
                <span>Итого:</span>
                <div class="flex-1 border-b border-dashed"></div>
                <b>{{ totalPrice }} p</b>
            </div>
            <div class="flex gap-1">
                <span>Налог 5%</span>
                <div class="flex-1 border-b border-dashed"></div>
                <b>{{ vatPrice }} p</b>
            </div>
            <button 
                :disabled="buttonDisabled"
                @click="createOrder"
                class="transition bg-lime-500 w-full rounded-xl py-3 disabled:bg-slate-400 text-white hover:bg-lime-600 active:bg-lime-700 cursor-pointer">
                Оформить заказ
            </button>
        </div>
    </div>
</template>