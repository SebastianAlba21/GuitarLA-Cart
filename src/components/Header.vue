<script setup>
import { computed } from 'vue';

const props = defineProps({
    carts: {
        type: Array,
        required: true
    },
    guitar: {
        type: Object,
        required: true
    }
})
defineEmits(['decrement-quantity', 'increment-quantity', 'add-to-cart', 'delete-guitar', 'empty-cart'])

const totalPayment = computed(() => {
    return props.carts.reduce((total, cart) => total + (cart.precio * cart.cantidad), 0)
})

</script>
<template>
    <header class="py-5 header">
        <div class="container-xl">
            <div class="row justify-content-center justify-content-md-between">
                <div class="col-8 col-md-3">
                    <a href="index.html">
                        <img class="img-fluid" src="../../public/img/logo.svg" alt="imagen logo" />
                    </a>
                </div>
                <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
                    <div class="carrito">
                        <img class="img-fluid" src="../../public/img/carrito.png" alt="imagen carrito" />

                        <div id="carrito" class="bg-white p-3">
                            <p class="text-center" v-if="carts.length === 0">El carrito esta vacio</p>
                            <div v-else>
                                <table class="w-100 table">
                                    <thead>
                                        <tr>
                                            <th>Imagen</th>
                                            <th>Nombre</th>
                                            <th>Precio</th>
                                            <th>Cantidad</th>
                                            <th></th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="cart in carts">
                                            <td>
                                                <img class="img-fluid" :src="'/img/' + cart.imagen + '.jpg'"
                                                    :alt="'Guitar: ' + cart.nombre" />
                                            </td>
                                            <td>{{ cart.nombre }}</td>
                                            <td class="fw-bold">${{ cart.precio }}</td>
                                            <td class="flex align-items-start gap-4">
                                                <button type="button" class="btn btn-dark"
                                                    @click="$emit('decrement-quantity', cart.id)">
                                                    -
                                                </button>
                                                <span class="user-select-none">{{ cart.cantidad }}</span>
                                                <button type="button" class="btn btn-dark"
                                                    @click="$emit('increment-quantity', cart.id)">
                                                    +
                                                </button>
                                            </td>
                                            <td>
                                                <button @click="$emit('delete-guitar', cart.id)" class="btn btn-danger"
                                                    type="button">
                                                    X
                                                </button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>

                                <p class="text-end">
                                    Total pagar: <span class="fw-bold">${{ totalPayment }}</span>
                                </p>
                                <button @click="$emit('empty-cart')" class="btn btn-dark w-100 mt-3 p-2">
                                    Vaciar Carrito
                                </button>
                            </div>
                        </div>
                    </div>
                </nav>
            </div>
            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold text-white">Modelo {{ guitar.nombre }}</h1>
                    <p class="mt-5 fs-5 text-white">
                        {{ guitar.descripcion }}
                    </p>
                    <p class="text-primary fs-1 fw-black">${{ guitar.precio }}</p>
                    <button @click="$emit('add-to-cart', guitar)" type="button"
                        class="btn fs-4 bg-primary text-white py-2 px-5">
                        Agregar al Carrito
                    </button>
                </div>
            </div>
        </div>

        <img class="header-guitarra" src="../../public/img/header_guitarra.png" alt="imagen header" />
    </header>
</template>