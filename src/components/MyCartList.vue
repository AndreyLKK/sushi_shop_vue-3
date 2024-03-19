<template>
  <div class="cart-wrapper" v-for="cartItem in cartsItem" :key="cartItem.id">
    <div class="cart-item" data-id="02">
      <div class="cart-item__top">
        <div class="cart-item__img">
          <img :src="require('@/assets/img/' + cartItem.image)" alt="" />
        </div>
        <div class="cart-item__desc">
          <div class="cart-item__title">{{ cartItem.title }}</div>
          <div class="cart-item__weight">
            {{ cartItem.items }} шт. / {{ cartItem.weight }}г.
          </div>

          <!-- cart-item__details -->
          <div class="cart-item__details">
            <div class="items items--small counter-wrapper">
              <button
                class="items__control"
                data-action="minus"
                @click="subtractQuantityInCart(cartItem)"
              >
                -
              </button>
              <div class="items__current" data-counter="">
                {{ cartItem.count }}
              </div>
              <button
                class="items__control"
                data-action="plus"
                @click="addQuantityInCart(cartItem)"
              >
                +
              </button>
            </div>

            <div class="price">
              <div class="price__currency">{{ cartItem.price }} ₽</div>
            </div>
          </div>
          <!-- // cart-item__details -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MyCartList",
  props: {
    cartsItem: {
      type: Array,
      required: true,
    },
    subtractQuantityInCart: {
      type: Function,
      required: true,
    },
    addQuantityInCart: {
      type: Function,
      required: true,
    },
    totalPrice: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {};
  },
  methods: {},
};
</script>

<style></style>
