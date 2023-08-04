<script setup>
  var productList = ref([]);

  var productNew = ref({
    name: "",
    price: 0,
    amount: 1,
  });
  function removeProductAmount(product) {
    if (product.amount > 0) {
      product.amount--;
    }
  }
  function increaseProductAmount(product) {
    product.amount++;
  }

  function addProductToList() {
    productList.value.push({ ...productNew.value });
    resetProductNew();
  }

  function resetProductNew() {
    productNew.value = {
      name: "",
      price: 0,
      amount: 1,
    };
  }

  function sumProductListPrices(productList) {
    return productList.reduce(
      (total, product) => (total += product.price * product.amount),
      0
    );
  }

  function removeProductFromList(productToRemove) {
    productList.value = productList.value.filter(
      (product) => product.name !== productToRemove.name
    );
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
        <button @click="addProductToList()">+</button>
      </div>
    </div>
  </div>
</template>

