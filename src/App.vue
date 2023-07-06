<script setup>
import './styles.css'
import { ref, onMounted, watch } from 'vue'
import { db } from './data/guitars.js'
import Header from './components/Header.vue'
import Guitar from './components/Guitar.vue'
import Footer from './components/Footer.vue'

const guitars = ref([])
const carts = ref([])
const guitar = ref({})

watch(carts, () => {
  saveLocalStorage()
}, { deep: true })

onMounted(() => {
  guitars.value = db
  guitar.value = db[3]
  const cartStorage = localStorage.getItem('carts')
  if (cartStorage) {
    carts.value = JSON.parse(cartStorage)
  }
})

const saveLocalStorage = () => {
  localStorage.setItem('carts', JSON.stringify(carts.value))
}

const addToCart = (guitar) => {
  const exists = carts.value.findIndex((cart) => cart.id === guitar.id)
  if (exists >= 0) {
    carts.value[exists].cantidad++
  } else {
    guitar.cantidad = 1
    carts.value.push(guitar)
  }
}

const decrementQuantity = (id) => {
  const index = carts.value.findIndex((cart) => cart.id === id)
  if (carts.value[index].cantidad <= 1) return
  carts.value[index].cantidad--

}

const incrementQuantity = (id) => {
  const index = carts.value.findIndex((cart) => cart.id === id)
  if (carts.value[index].cantidad >= 9) return
  carts.value[index].cantidad++

}

const deleteGuitar = (id) => {
  const index = carts.value.findIndex((cart) => cart.id === id)
  carts.value.splice(index, 1)
}

const emptyCart = () => {
  carts.value = []
}

</script>
<template>
  <Header :carts="carts" :guitar="guitar" @decrement-quantity="decrementQuantity" @increment-quantity="incrementQuantity"
    @add-to-cart="addToCart" @delete-guitar="deleteGuitar" @empty-cart="emptyCart" />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>
    <div class="row mt-5">
      <Guitar v-for="guitar in guitars" :key="guitar.id" :guitar="guitar" @add-to-cart='addToCart' />
    </div>
  </main>
  <Footer />
</template>
