<template>
  <header class="header">
    <div class="container text-center">
      <img src="./assets/img/logo/logo.png" width="92" alt="Суши и пицца" />
      <h3 class="display-4">Доставка роллов</h3>
      <p class="lead">Оперативно и вкусно</p>
    </div>
  </header>

  <div class="container mb-5">
    <div class="row-wrapper">
      <div class="col-md-8">
        <div class="row">
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
                  @click="addCart(card)"
                >
                  + в корзину
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-md-4">
        <div class="card mb-4">
          <div class="card-body">
            <h4 class="card-title">Ваш заказ</h4>
            <div
              data-cart-empty
              class="alert alert-secondary"
              role="alert"
              v-if="cartsItem.length === 0"
            >
              Корзина пуста
            </div>

            <div
              class="cart-wrapper"
              v-for="cartItem in cartsItem"
              :key="cartItem.id"
            >
              <div class="cart-item" data-id="02">
                <div class="cart-item__top">
                  <div class="cart-item__img">
                    <img
                      :src="require('./assets/img/' + cartItem.image)"
                      alt=""
                    />
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
                        <div class="price__currency">
                          {{ cartItem.price }} ₽
                        </div>
                      </div>
                    </div>
                    <!-- // cart-item__details -->
                  </div>
                </div>
              </div>
            </div>
            <div class="cart-total">
              <p class="card-text" v-if="totalPrice !== 0">
                <span class="h5">Доставка:</span>
                <span class="delivery-cost free">
                  {{ deliveryStatus }}
                </span>
              </p>
              <p>
                <span class="h5">Итого:</span>
                <span class="total-price">{{ totalPrice }}</span>
                <span class="rouble">₽</span>
              </p>
            </div>
          </div>

          <div id="order-form" class="card-body border-top">
            <h4 class="card-title">Оформить заказ</h4>
            <form>
              <div class="form-group">
                <input
                  type="text"
                  class="form-control"
                  placeholder="Ваш номер телефона"
                />
              </div>
              <button type="submit" class="btn btn-primary">Заказать</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      cards: [
        {
          count: 1,
          id: "01",
          image: "california-hit.jpg",
          title: "Филадельфия хит ролл",
          items: "6",
          weight: "180",
          price: "300",
        },
        {
          count: 1,
          id: "02",
          image: "california-tempura.jpg",
          title: "Калифорния темпура",
          items: "6",
          weight: "205",
          price: "250",
        },
        {
          count: 1,
          id: "03",
          image: "philadelphia.jpg",
          title: "Запеченый ролл «Калифорния»",
          items: "6",
          weight: "182",
          price: "230",
        },
        {
          count: 1,
          id: "04",
          image: "zapech-california.jpg",
          title: "Филадельфия",
          items: "6",
          weight: "23",
          price: "320",
        },
      ],
      cartsItem: [],
      semElement: [],
      totalPrice: 0,
      deliveryStatus: "Доставка бесплатная",
    };
  },
  methods: {
    addCart(card) {
      const existingCartItem = this.cartsItem.find(
        (item) => item.id === card.id
      );

      if (existingCartItem) {
        existingCartItem.count += card.count;
      } else {
        const cartItem = Object.assign({}, card);
        this.cartsItem.push(cartItem);
      }
      this.semElement = [];
      this.cartsItem.forEach((item) => {
        let res = item.price * item.count;
        item.isInCart = true;
        this.semElement.push(res);
      });
      this.totalPrice = this.semElement.reduce((accum, item) => {
        return (accum += item);
      }, 0);
      card.count = 1;
    },

    addQuantity(card) {
      card.count++;
    },

    addQuantityInCart(cartItem) {
      cartItem.count++;
      const isInCart = this.cartsItem.some((item) => item.id === cartItem.id);

      if (isInCart) {
        this.totalPrice += Number(cartItem.price);
      }
    },

    subtractQuantity(card) {
      if (card.count < 2) {
        card.count;
      } else {
        card.count--;
      }
    },

    subtractQuantityInCart(cartItem) {
      if (cartItem.count < 2) {
        const index = this.cartsItem.findIndex(
          (item) => item.id === cartItem.id
        );
        this.cartsItem.splice(index, 1);
        this.totalPrice -= Number(cartItem.price);

        if (this.cartsItem.length === 0) {
          this.totalPrice = 0;
        }
      } else {
        cartItem.count--;
        const isInCart = this.cartsItem.some((item) => item.id === cartItem.id);

        if (isInCart) {
          this.totalPrice -= Number(cartItem.price);
        }
      }
    },
  },

  watch: {
    totalPrice(newTotalPrice) {
      if (newTotalPrice > 600) {
        this.deliveryStatus = "Доставка бесплатная";
      } else {
        this.deliveryStatus = "600 р";
      }
    },
  },
};
</script>

<style>
body,
html {
  background-color: #f8f9f9;
  height: 100%;
}

h3,
h4,
p {
  font-family: "Merriweather", serif;
}

.hidden {
  display: none;
}

button {
  padding: 0;
  margin: 0;
  border: none;
  background: none;
  font: inherit;
  color: inherit;
  cursor: pointer;
}

.header {
  padding-top: 50px;
  padding-bottom: 50px;
  text-align: center;
}

.product-img {
  width: 265px;
  height: auto;
  margin-left: auto;
  margin-right: auto;
}

.item-title {
  font-size: 1.5rem;
  font-weight: 500;
  font-family: "Merriweather", serif;
  height: 70px;
}

.details-wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 25px;
  width: 80%;
  margin-left: auto;
  margin-right: auto;
}

.items {
  font-family: "Merriweather", serif;
  background: #f2ede7;
  border-radius: 8px;
  width: 120px;
  display: flex;
  font-size: 18px;
  height: 30px;
  overflow: hidden;
}

.items__control {
  width: 40px;
  cursor: pointer;
  transition: 0.2s ease-in;
  text-align: center;
}

.items__control:hover {
  background: #eb5a1e;
  color: #fff;
}

.items__current {
  width: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* items--small */
.items.items--small {
  width: 90px;
  font-size: 16px;
  height: 26px;
  text-align: center;
}

.items.items--small .items__control {
  width: 30px;
}

.items.items--small .items__current {
  width: 30px;
}

.price {
  text-align: left;
}

.price__weight {
  color: #6c757d !important;
  font-size: 80%;
  line-height: 1;
}

.price__currency {
  font-size: 1.5rem;
  font-weight: 600;
  line-height: 1;
}

.btn-outline-warning {
  color: #eb5a1e;
  border-color: #eb5a1e;
}

.btn-outline-warning:hover {
  color: #fff;
  background-color: #eb5a1e;
  border-color: #eb5a1e;
}

.cart-item {
}

.cart-item__top {
  display: flex;
  align-items: flex-start;
  padding-bottom: 15px;
  border-bottom: 1px solid #dee2e6;
  margin-bottom: 15px;
}

.cart-item__img {
}

.cart-item__img img {
  max-width: 100px;
  height: auto;
}

.cart-item__desc {
  padding-top: 15px;
  padding-left: 15px;
}

.cart-item__title {
  font-family: "Merriweather", serif;
  font-size: 16px;
  line-height: 1.2;
  font-weight: 600;
  margin-bottom: 5px;
}

.cart-item__weight {
  color: #6c757d !important;
  font-size: 80%;
  line-height: 1;
  margin-bottom: 15px;
}

.cart-item__details {
  display: flex;
  justify-content: flex-start;
  align-items: center;
}

.cart-item__details .price__currency {
  font-size: 18px;
  margin-left: 15px;
}

.cart-total {
  font-size: 18px;
  font-weight: 500;
}

.delivery-cost {
}

.free {
  color: #0ea107;
}

.total-price {
  color: #eb5a1e;
  font-weight: bold;
  margin-left: 10px;
}

.rouble {
  color: #eb5a1e;
}

.none {
  display: none;
}

.header {
  background-color: #f8f9fa;
  padding: 20px 0;
}

.header .container {
  text-align: center;
}

.header img {
  width: 92px;
}

.header h3 {
  font-size: 2.5rem;
  margin-top: 10px;
  margin-bottom: 5px;
}

.header p {
  font-size: 1.25rem;
  color: #6c757d;
}

.mb-5 {
  margin-bottom: 3rem;
}

.row-wrapper {
  display: flex;
  justify-content: space-around;
}

.row {
  margin-right: -15px;
  margin-left: -15px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 20px;
}

.col-md-8 {
  flex: 0 0 66.666667%;
  max-width: 66.666667%;
}

.col-md-4 {
  width: 400px;
}

.card {
  position: relative;
  display: flex;
  flex-direction: column;
  min-width: 0;
  word-wrap: break-word;
  background-color: #fff;
  background-clip: border-box;
  border: 1px solid rgba(0, 0, 0, 0.125);
  border-radius: 0.25rem;
}

.card-body {
  flex: 1 1 auto;
  padding: 1.25rem;
}

.card-title {
  margin-bottom: 0.75rem;
  font-size: 1.25rem;
}

.card-img-top {
  width: 100%;
  border-top-left-radius: calc(0.25rem - 1px);
  border-top-right-radius: calc(0.25rem - 1px);
}

.text-center {
  text-align: center !important;
}

.btn {
  display: inline-block;
  font-weight: 400;
  color: #212529;
  text-align: center;
  vertical-align: middle;
  user-select: none;
  background-color: transparent;
  border: 1px solid transparent;
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  line-height: 1.5;
  border-radius: 0.25rem;
  transition: color 0.15s, background-color 0.15s, border-color 0.15s,
    box-shadow 0.15s;
}

.btn-block {
  width: 100%;
}

.btn-outline-warning {
  color: #ffc107;
  border-color: #ffc107;
}

.btn-primary {
  color: #fff;
  background-color: #007bff;
  border-color: #007bff;
}

.form-control {
  display: block;
  width: 100%;
  padding: 5px;
  font-size: 1rem;
  line-height: 1.5;
  color: #495057;
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
  margin-bottom: 20px;
}

.alert {
  position: relative;
  padding: 0.75rem 1.25rem;
  margin-bottom: 1rem;
  border: 1px solid transparent;
  border-radius: 0.25rem;
}

.alert-secondary {
  color: #6c757d;
  background-color: #f8f9fa;
  border-color: #e2e3e5;
}

.hidden {
  display: none !important;
}

.card-total {
  margin-top: 1rem;
}

.rouble {
  font-weight: bold;
}

.border-top {
  border-top: 1px solid #dee2e6 !important;
}

#order-form {
  margin-top: 1rem;
  padding-top: 1rem;
  border-top: 1px solid #e9ecef;
}

.order-form .form-group {
  margin-bottom: 1rem;
}
</style>
