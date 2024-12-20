<script setup>
import { onMounted, provide, reactive, ref, watch } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'

const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://f32b821cc4008a80.mokky.dev/favorites`)

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
      }
      item.isFavorite = true

      const { data } = await axios.post(`https://f32b821cc4008a80.mokky.dev/favorites`, obj)

      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://f32b821cc4008a80.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

const fetchItem = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(`https://f32b821cc4008a80.mokky.dev/sneakers`, {
      params
    })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  await fetchItem()
  await fetchFavorites()
})
watch(filters, fetchItem)

provide('addToFavorite', addToFavorite)
</script>

<template>
  <div>
    <!-- <Drawer /> -->

    <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
      <Header />

      <div class="px-10">
        <div class="flex justify-between items-center">
          <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

          <div class="flex gap-4">
            <select @change="onChangeSelect" class="py-2 px-3 border rounded-mb outline-none">
              <option value="name">По названию</option>
              <option value="price">По цене ( дешевые)</option>
              <option value="-price">По цене ( дорогие)</option>
            </select>

            <div class="relative">
              <img src="/search.svg" class="absolute left-3 top-3" />
              <input
                @input="onChangeSearchInput"
                class="border border-gray-200 rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
                type="text"
                placeholder="Поиск..."
              />
            </div>
          </div>
        </div>

        <div class="mt-10">
          <CardList :items="items" @addToFavorite="addToFavorite" />
        </div>
      </div>
    </div>
  </div>
</template>
