<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  product: Object
})

const emit = defineEmits(['update-quantity'])
const quantity = ref(0)

watch(quantity, (newVal) => {
  emit('update-quantity', props.product, newVal)
})
</script>

<template>
  <div class="product-card" style="display: flex; justify-content: space-between;">
    <div class="product" style="text-align: left;">
        <h3 style="font-size: x-large; margin-bottom: 5px;">{{ product.name }}</h3>
        <p style="color: greenyellow; font-size: larger; font-weight: 600; margin-top: 0px;">${{ product.price }}</p>
    </div>
    <q-btn v-show="quantity === 0" style="margin: auto 0;" @click="quantity = 1" label="Agregar al Carrito"/>
    <div v-show="quantity > 0" class="hasAmount" style="display: flex; margin: auto 0;">
        <q-btn round @click="quantity--" 
            style="background-color: #ff3333;
            height: 20px; width: 20px;" 
            label="-"/>
        <p style="margin: auto 20px; font-size: large;">{{ quantity }}</p>
        <q-btn round @click="quantity++" 
            style="background-color: #33ff33;
            height: 20px; width: 20px;
            " 
            label="+"/>
    </div>
  </div>
</template>


<style scoped>
</style>