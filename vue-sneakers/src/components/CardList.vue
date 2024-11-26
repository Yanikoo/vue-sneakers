<script setup>
import Card from './Card.vue'

defineProps({
  items: Array
})
const emit = defineEmits(['addToFavorite'])

const onClickAdd = () => {
  alert('Добавлено в корзину')
}

const addToFavorite = inject('addToFavorite')
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
      :onClickFavorite="() => addToFavorite(item)"
      :isFavorite="item.isFavorite"
    />
  </div>
</template>
