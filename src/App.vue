<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <div class="main">
      <h2 class="cart">Cart({{ cart.length }})</h2>
  </div>

  <Product :vipClient = "vipClient" :cart="cart" @add-to-cart="updateCart" @remove-from-cart="updateCartt"/>
  <Product />
  <Product />
</template>

<script>
import Product from './components/Product.vue';

export default {
  name: 'App',
  data() {
    return {
      cart: [],
      vipClient: true,
    }
  },
  methods: {
    updateCart(id) {
      this.cart.push(id);
    },
    updateCartt(id) {
      const index = this.cart.findIndex(el => el === id);
      console.log('indeksas ', index, ' id: ', id );
      this.cart.splice(index, 1);
    }
  },
  components: {
    Product
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>


<style>
.main {
  display: inline-block;
  width: 100%;
}
.cart {
    display: block;
  margin: 25px 100px;
  float: right;
  border: 1px solid #d8d8d8;
  padding: 10px 30px;
  background-color: white;
  -webkit-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 2px 15px -12px rgba(0, 0, 0, 0.57);
}
</style>