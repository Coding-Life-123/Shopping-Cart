<script setup>
import { computed, ref, watch } from 'vue'
import Product from './Product.vue'
import { useQuasar } from 'quasar';

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

const totalItems = ref([])

function updateCart(product, quantity) {
  const existing = totalItems.value.find(p => p.id === product.id)

  if (quantity > 0) {
    if (existing) {
      existing.quantity = quantity
    } else {
      totalItems.value.push({ ...product, quantity })
    }
  } else {
    totalItems.value = totalItems.value.filter(p => p.id !== product.id)
  }
}

const subtotal = computed(()=>{
  return totalItems.value.reduce((acc, p)=>{
    return acc + (p.price * p.quantity)
  }, 0)
})

const impuesto = computed(()=>{
  return Math.round((subtotal.value*0.16)*100)/100
})

const total = computed(()=>{
  return Math.round((subtotal.value + impuesto.value)*100)/100
})

const $q = useQuasar()
const alertCart = ref(false)

watch(total, (newVal, oldVal)=>{
  if(newVal>1000 && alertCart.value == false){
    $q.notify({
      message:`Tu carrito supera los $1000. ¡Podrías calificar para envío gratis!`,
      color: 'yellow-9',
      position: 'top',
      timeout: 2000
    })

    alertCart.value = true
  }else if(newVal < 1000){
    alertCart.value = false
  }
})

let timer = null

watch(totalItems, (newVal, oldVal)=>{
  if(newVal!==oldVal){
    clearTimeout(timer)

    timer = setTimeout(() => {        
      localStorage.setItem('carrito', JSON.stringify(newVal));
      $q.notify({
        message:`Se actualizó el carrito en el localStorage`,
        color: 'green-6',
        position: 'top',
        timeout: 2000
      })
    }, 800);
  }
})


const carritoGuardado = ref(JSON.parse(localStorage.getItem('carrito')))
console.log(carritoGuardado.value)


watch(totalItems, (newVal, oldVal)=>{
  const carritoGuardado = ref(JSON.parse(localStorage.getItem('carrito')))

  totalItems.value = carritoGuardado.value;
  if(totalItems.value.length == 0 && carritoGuardado.value.length !== 0){
    $q.notify({
      message:`Se actualizó el carrito actual con los valores encontrados localmente`,
      color: 'grey-6',
      position: 'bottom-right',
      timeout: 2000
    })
  }else{
    $q.notify({
      message:`No se encontró ningún carrito guardado`,
      color: 'grey-6',
      position: 'bottom-right',
      timeout: 2000
    })
  }
})

</script>

<template>
  <div class="main-container" style="padding: 20px 60px; z-index: 20; width: 60%; margin: 30px auto; display: flex; flex-direction: column; border-radius: 20px; background: linear-gradient(200deg, var(--third), var(--fourth)); ">
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
    <hr style="width: 100%;"></hr>
    <ul>
      <li style="display: flex; justify-content: space-between; font-size: 18px;" v-for="p in totalItems" :key="p.id">
        <p>x{{ p.quantity }} {{ p.name }}</p> <p style="color: greenyellow; font-weight: 600;">${{ p.quantity*p.price }}</p>
      </li>
    </ul>
    <hr style="width: 100%; margin-bottom: 20px;"></hr>
    <div style="display: flex; justify-content: space-between; font-size: 16px; width: 100%;"><p>Subtotal:</p> <p style="color: yellowgreen; font-weight: 600;">${{ subtotal }}</p></div>
    <div style="display: flex; justify-content: space-between; font-size: 16px; width: 100%;"><p>Impuesto:</p> <p style="color: yellowgreen; font-weight: 600;">${{ impuesto }}</p></div>
    <hr style="width: 100%; margin-bottom: 20px;">
    <div style="display: flex; justify-content: space-between; font-size: 20px; width: 100%;"><p>Total:</p> <p style="color: yellowgreen; font-weight: 600;">${{ total }}</p></div>
  </div>
</template>
