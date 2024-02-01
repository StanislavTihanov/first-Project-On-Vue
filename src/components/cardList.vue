<script setup>
    import card from './card.vue'

    defineProps({
        items: Array,
    })

    const emit = defineEmits (['addToFavorite', 'addToCart']);

</script>

<template>
    <div class="grid grid-cols-4 gap-5 items" v-auto-animate>

        <card   
            v-for="item in items"
            :key="item.id"
            :id="item.id"
            :title="item.title" 
            :imageUrl="item.imageUrl" 
            :price="item.price"
            :is-added="false"
            :is-favorite="false"
            :isFavorite="item.isFavorite"
            :onClickFavorite="() => emit('addToFavorite', item)"
            :onClickAdd="() => emit('addToCart', item)"
            :isAdded="item.isAdded"
        />
        
    </div>
</template>