<template>
  <BookDetails v-if="isShowBookDetails" :book="bookModel.book" />
  <div v-if="isShowNoBookMessage" class="text-center mt-10">Не нашли книгу</div>
</template>

<script setup lang="ts">
import { useRoute } from 'vue-router'
import { books_v1 } from '@googleapis/books/v1'
import { BookDetails } from '@/widgets/book-details/'
import { useBookModel } from '@/entities/book'

const route = useRoute()
const bookModel = useBookModel()

const isShowNoBookMessage = ref(false)

onMounted(() => {
  getBook()
})

onBeforeUnmount(() => {
  bookModel.$reset()
})

const isShowBookDetails = computed(
  (): boolean => Object.keys(bookModel.book as books_v1.Schema$Volume).length > 0
)

const getBook = async () => {
  const { id } = route.params

  if (typeof id === 'string') {
    const { error } = await bookModel.fetchBookById(id)

    if (error) {
      isShowNoBookMessage.value = true
    }
  }
}
</script>
