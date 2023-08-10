<script setup>
  // 👉 Props
  const props = defineProps({ product: Object, prop: String });

  // 🔥 Functions
  function descreaseCounter(product, prop) {
    if (product[prop] > 0) {
      product[prop]--;
    }
  }
  function increaseCounter(product, prop) {
    product[prop]++;
  }
</script>

<template>
  <div class="product__amount">
    <input type="number" v-model.number="product[prop]" placeholder="Qtd" />

    <div class="button__remove">
      <div class="p1">
        <button @click="descreaseCounter(product, prop)">-</button>
      </div>
    </div>

    <div class="button__add">
      <div class="p1">
        <button @click="increaseCounter(product, prop)">+</button>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
