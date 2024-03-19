<template>
  <div class="col-md-6" v-for="card in cards" :key="card.id">
    <div class="card mb-4" :data-id="card.id">
      <img
        class="product-img"
        :src="require('@/assets/img/' + card.image)"
        alt=""
      />
      <div class="card-body text-center">
        <h4 class="item-title">{{ card.title }}</h4>
        <p>
          <small data-items-in-box class="text-muted"
            >{{ card.items }} шт.</small
          >
        </p>

        <div class="details-wrapper">
          <div class="items counter-wrapper">
            <button
              class="items__control"
              data-action="minus"
              @click="subtractQuantity(card)"
            >
              -
            </button>
            <div class="items__current" data-counter>
              {{ card.count }}
            </div>
            <button
              class="items__control"
              data-action="plus"
              @click="addQuantity(card)"
            >
              +
            </button>
          </div>

          <div class="price">
            <div class="price__weight">{{ card.weight }}г.</div>
            <div class="price__currency">{{ card.price }}</div>
          </div>
        </div>

        <button
          type="button"
          class="btn btn-block btn-outline-warning"
          @click="addToCart(card)"
        >
          + в корзину
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MyItemList",
  props: {
    cards: {
      type: Array,
      required: true,
    },
    addToCart: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {};
  },
  methods: {
    addQuantity(card) {
      card.count++;
    },

    subtractQuantity(card) {
      if (card.count < 2) {
        card.count;
      } else {
        card.count--;
      }
    },
  },
};
</script>

<style></style>
