<script setup>
import Card from './Card.vue'
 

defineProps({
    items: Array
})


const onClickAdd = () => {
    alert('Добавлено в корзину')
}
</script>

<template>
    <div class="grid grid-cols-4 gap-5 mt-8">
        <Card 
            v-for="item in items"
            :key="item.id"
            :title="item.title"
            :image-url="item.imageUrl"
            :price="item.price"
            :is-favorite="false"
            :onClickAdd="onClickAdd"
        />
    </div>
    
</template>