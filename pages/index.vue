<script setup>
  var productList = ref([]);

  var productNew = ref({
    name: null,
    price: null,
    amount: null,
  });
  function removeProductAmount(product) {
    if (product.amount > 0) {
      product.amount -= 1;
    }
  }
  function increaseProductAmount(product) {
    if (product.amount === null) {
      product.amount = 1;
    } else {
      product.amount += 1;
    }
  }
  function getCopy(productNew) {
    return JSON.parse(JSON.stringify(productNew));
  }

  function addProductToList(productNew, productList) {
    productList.push(getCopy(productNew));
    resetProductNew(productNew);
  }
  function resetProductNew(productNew) {
    productNew.name = null;
    productNew.price = null;
    productNew.amount = null;
  }

  function sumProductListPrices(productList) {
    return productList.reduce((total, product) => {
      return (total += product.price * product.amount);
    }, 0);
  }

  function removeProductFromList(product, productList) {
    const i = productList.findIndex((p) => p.name === product.name);
    productList.splice(i, 1);
  }
</script>

<template>
  <div class="app">
    <div class="products__list">
      <div class="products__list__header">
        <div>Produto</div>
        <div>Preço</div>
        <div>Qtd</div>
        <div>Total</div>
        <div>Remover</div>
      </div>
      <div class="products__list__content">
        <div class="product" v-for="product in productList" :key="product.name">
          <div>
            {{ product.name }}
          </div>
          <div>
            {{ product.price }}
          </div>
          <div class="counter">
            <div @click="removeProductAmount(product)">-</div>
            <div>{{ product.amount }}</div>
            <div @click="increaseProductAmount(product)">+</div>
          </div>
          <div>
            {{ product.price * product.amount }}
          </div>
          <div>
            <button
              class="product__list__remove"
              @click="removeProductFromList(product, productList)"
            >
              X
            </button>
          </div>
        </div>
      </div>
    </div>

    <div class="price__total">
      Total {{ sumProductListPrices(productList) }}
    </div>

    <!-- Formuário de Produto -->
    <div class="product__form">
      <div>
        <input
          class="product__name"
          v-model="productNew.name"
          placeholder="Produto"
        />
      </div>
      <div class="product__price">
        <input
          type="number"
          v-model.number="productNew.price"
          placeholder="Preço"
        />
      </div>
      <div class="product__amount">
        <input
          type="number"
          v-model.number="productNew.amount"
          placeholder="Qtd"
        />
        <button class="button__remove" @click="removeProductAmount(productNew)">
          -
        </button>
        <button class="button__add" @click="increaseProductAmount(productNew)">
          +
        </button>
      </div>
      <div class="product__add">
        <button @click="addProductToList(productNew, productList)">+</button>
      </div>
    </div>
  </div>
</template>

<style>
  *:not(head, style, script) {
    margin: 0;
    padding: 0;
    width: stretch;
    float: left;
    display: block;
    position: relative;
    box-sizing: border-box;
    font-size: 1rem;
    font-family: "Poppins";
    border: none;
    /* box-shadow: 0 0 0 2px #c300ff; */
  }
  html,
  body,
  #__nuxt,
  #__nuxt > div {
    height: 100vh;
    overflow: hidden;
  }

  .product {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
    gap: 0.5rem;
    padding: 0.5rem;
    align-content: center;
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  }
  .product > div {
    padding: 0.5rem;
  }

  .app {
    height: 100%;
    display: grid;
    grid-template-rows: 5fr min-content 4fr;
  }
  input,
  button {
    padding: 1rem;
    width: 100%;
  }

  .price__total {
    background-color: #111;
    padding: 1rem;
    color: #fff;
  }

  .product__form {
    width: 100%;
    padding: 1rem;
    background-color: #eee;
  }
  .product__form > div {
    padding: 0.5rem;
  }
  .product__form button,
  .product__form input {
    border-radius: 1rem;
  }
  .products__list {
    height: 100%;
    overflow: scroll;
    background-color: rgb(255, 255, 255);
  }
  .products__list__header {
    position: sticky;
    top: 0;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
    gap: 0.5rem;
    align-content: center;
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
    position: sticky;
    padding: 0.5rem;
    background-color: #fff;
    z-index: 1;
  }
  .products__list__header div {
    padding: 0.5rem;
  }
  .product__price,
  .product__amount {
    width: 50%;
  }
  .product__amount input {
    text-align: center;
  }

  .product__amount .button__remove,
  .product__amount .button__add {
    width: 48px;
    height: 48px;
    position: absolute;
  }

  .product__amount .button__add {
    right: 0%;
  }

  .product__list__remove {
    width: fit-content;
    height: fit-content;
    padding: 0.5rem;
    border-radius: 4rem;
  }
  .product__add > button {
    background-color: blueviolet;
    color: #fff;
  }
  .counter > div {
    width: 33.33%;
    text-align: center;
  }
</style>
