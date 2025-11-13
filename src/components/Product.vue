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
  <div class="product-card" :class="{'active': quantity > 0, 'product-card': quantity == 0}" style="display: flex; justify-content: space-between; border: 1px solid white; padding: 0px 20px; margin: 10px 0px; border-radius: 10px;">
    <div class="product" style="text-align: left;">
        <h3 style="font-size: x-large; margin-bottom: 5px;">{{ product.name }}</h3>
        <p style="color: greenyellow; font-size: larger; font-weight: 600; margin-top: 0px;">${{ product.price }}</p>
    </div>
    <q-btn v-show="quantity === 0" style="margin: auto 0; background-color: aquamarine; color: darkslategray; font-weight: 600; border-radius: 14px;" @click="quantity = 1" label="Agregar al Carrito"/>
    <div v-show="quantity > 0" class="hasAmount" style="display: flex; margin: auto 0;">
        <q-btn round @click="quantity--" 
            style="background-color: #bf0000;
            height: 10px; width: 10px;
            font-weight: 600; font-size: large;" 
            label="-"/>
        <p style="margin: auto 20px; font-size: large;">{{ quantity }}</p>
        <q-btn round @click="quantity++" 
            style="background-color: #00bf00;
            padding: 5px;
            height: 5px; width: 5px;
            font-weight: 600; font-size: large;
            " 
            label="+"/>
    </div>
  </div>
</template>


<style scoped>

  .product-card{
    position: relative;
    border: 1px solid white;
    background: transparent;
    overflow: hidden;
  }

  .product-card::before{
    content: "";
    position: absolute;
    inset: 0;
    background: linear-gradient(90deg, transparent 20%, #fff3 70%, #fff8 100%);
    width: 0%;
    right: 0;
    left: auto;
    transition: width 1s ease;
    z-index: -1;
  }

  .product-card.active::before{
    width: 100%;
  }

</style>