<script setup>
    import { ref, onMounted } from 'vue'
    import axios from 'axios'
    import cardList from '../components/cardList.vue'

const favorites = ref([])

onMounted( async() => {
    try{
        const {data} = await axios.get('https://3af0cc7479d9596f.mokky.dev/favorites?_relations=');
        favorites.value = data.map((obj) => obj.item);
    } catch (err) {
        console.log(err)
    }
});
</script>


<template>
    <h1>Мои закладки</h1>
    <cardList :items="favorites"/>
</template>