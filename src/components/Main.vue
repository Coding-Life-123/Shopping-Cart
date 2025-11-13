<script setup>
import { ref } from 'vue'
import Product from './Product.vue'

const products = [
  { id: 1, name: 'Laptop Dell', price: 899 },
  { id: 2, name: 'Smartphone Samsung Galaxy', price: 699 },
  { id: 3, name: 'Auriculares Sony WH-1000XM5', price: 129 },
  { id: 4, name: 'Monitor LG 27" 1440p', price: 249 },
  { id: 5, name: 'Teclado mecánico', price: 89 },
  { id: 6, name: 'Ratón Logitech MX', price: 49 },
  { id: 7, name: 'Tablet Apple iPad', price: 599 },
  { id: 8, name: 'SSD Samsung 1TB', price: 119 },
  { id: 9, name: 'Impresora HP Todo-en-Uno', price: 159 },
  { id: 10, name: 'Cámara Canon EOS', price: 549 },
  { id: 11, name: 'Smartwatch Fitbit', price: 179 },
  { id: 12, name: 'Mochila Xiaomi para portátil', price: 39 },
  { id: 13, name: 'Altavoz JBL Bluetooth', price: 99 },
  { id: 14, name: 'Micrófono USB Condensador', price: 69 },
  { id: 15, name: 'Router TP-Link AC1200', price: 79 }
];

const selectedProducts = ref([])

function updateCart(product, quantity) {
  const existing = selectedProducts.value.find(p => p.id === product.id)

  if (quantity > 0) {
    if (existing) {
      existing.quantity = quantity
    } else {
      selectedProducts.value.push({ ...product, quantity })
    }
  } else {
    selectedProducts.value = selectedProducts.value.filter(p => p.id !== product.id)
  }
}

</script>

<template>
  <div class="main-container" style="padding: 20px; z-index: 20; width: 60%; margin: 0 auto; display: flex; flex-direction: column; border-radius: 20px; background: linear-gradient(200deg, var(--q-third), var(--q-fourth)); ">
    <h1 style="font-size: 50px; margin: 20px auto;">🛒 Carrito de Compras</h1>

    <div class="products">
      <Product
        v-for="product in products"
        :key="product.id"
        :product="product"
        @update-quantity="updateCart"
      />
    </div>

    <h2>Productos Seleccionados:</h2>
    <ul>
      <li v-for="p in selectedProducts" :key="p.id">
        {{ p.name }} - Cantidad: {{ p.quantity }}
      </li>
    </ul>
  </div>
</template>
