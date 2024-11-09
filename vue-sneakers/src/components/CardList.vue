<script setup>
import { inject } from 'vue'
import Card from './Card.vue'
 

defineProps({
    items: Array
})

const addToFavorite = inject('addToFavorite')

const onClickFavorite = () => {
  const obj = {
    ...props,
    parentId: props.id 
  }

  addToFavorite(obj)
}

const onClickAdd = () => {
    alert('Добавлено в корзину')
}
</script>

<template>
    <div class="grid grid-cols-4 gap-5 mt-8">
        <Card 
            v-for="item in items"
            :key="item.id"
            :id="item.id"
            :title="item.title"
            :image-url="item.imageUrl"
            :price="item.price"
            :onClickAdd="onClickAdd"
            :isFavorite="item.isFavorite"
        />
    </div>
    
</template>