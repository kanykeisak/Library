<template>
  <v-btn prepend-icon="mdi-bookshelf" :color="color" @click="bookModel.toggleBookshelfBook(book)">
    {{ text }}
  </v-btn>
</template>

<script setup lang="ts">
import { useBookModel, type IBook } from '@/entities/book'

const props = defineProps<{
  book: IBook
}>()
const bookModel = useBookModel()

const text = computed((): string =>
  bookModel.isBookInBookshelf(props.book.id) ? 'В библиотеке' : 'В библиотеку'
)

const color = computed((): string => (bookModel.isBookInBookshelf(props.book.id) ? 'green' : ''))
</script>
