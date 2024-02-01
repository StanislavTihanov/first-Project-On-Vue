
<script setup>
    import { reactive, watch, ref, onMounted } from 'vue'
    import cardList from '../components/cardList.vue'
    import axios from 'axios';
    import { inject } from 'vue';

const items = ref([]);


const { cart, addToCart, removeFromCart } = inject('cart')


const filters = reactive({
    sortBy: 'title',
    searchQuery: '',
});

const onClickAddPlus = (item) => {
    if (!item.isAdded) {
        addToCart(item);
    } else {
        removeFromCart(item);
    }
    }

const onChangeSelect = (event) => {
        filters.sortBy = event.target.value
    }

const onChangeSearchInput = (event )=> {
        filters.searchQuery = event.target.value
    }

const addToFavorite = async (item) => {
    try {
        if(!item.isFavorite) {
            const obj = {
            item_id: item.id,
        }
        item.isFavorite = true

        const {data} = await axios.post(`https://3af0cc7479d9596f.mokky.dev/favorites`, obj);
        item.favoriteId = data.id

        } else {
        item.isFavorite = false;
        await axios.delete(`https://3af0cc7479d9596f.mokky.dev/favorites/${item.favoriteId}`);
        item.favoriteId = null;
    }

    } catch (err) {
        console.log(err)
    }
}
const fetchFavorites = async () => {
    try {
    const {data: favorites} = await axios.get('https://3af0cc7479d9596f.mokky.dev/favorites');
    
    items.value = items.value.map(item => {
        const favorite = favorites.find(favorites => favorites.item_id === item.id);

        if (!favorites) {
            return item;
        }
        return {
            ...item,
            isFavorites: true,
            favoriteId: favorite.id,
        }
    });

    } catch (err) {
        console.log(err)
    }
}

const fetchItems = async () => {
    try {

    const params = {
        sortBy: filters.sortBy
    }
    
    if (filters.searchQuery) {
        params.title = `*${filters.searchQuery}*`
    }

    const {data} = await axios.get('https://85f56a38a16c6257.mokky.dev/items', {
        params
    });

    items.value = data.map((obj) => ({
        ...obj,
        isFavorites: false,
        favoriteId: null,
        isAdded: false
    }))
    } catch (err) {
    console.log(err);
    }
    }

onMounted (async () => {
    const localCart = localStorage.getItem('cart');
    cart.value = localCart ? JSON.parse(localCart) : [];

    await fetchItems();
    await fetchFavorites();

    items.value = items.value.map((item) => ({
        ...item,
        isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
    }))
});

watch(cart, () => {
    items.value = items.value.map((item) => ({
        ...item,
        isAdded: false
    }));
});

watch(filters, fetchItems);
</script>

<template>
<div class="flex justify-between items-center mb-5 flex-wrap">

    <h2 class="text-3xl font-bold mb-5">Все кросовки</h2>
    
    <div class="flex items-center gap-4 flex-wrap">
        <select @change="onChangeSelect" class="border rounded-md py-2.5 px-3 outline-none cursor-pointer">
            <option value="name">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>
        </select>
    
        <div class="relative">
            <img class="absolute top-3 left-4" src="/search.svg" alt="icon search">
            <input @input="onChangeSearchInput" class="border rounded-md py-2 pl-12 pr-4 outline-none focus:border-gray-400" placeholder="Поиск..." type="text"/>
        </div>
    </div>
</div>

<cardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus"/>
</template>