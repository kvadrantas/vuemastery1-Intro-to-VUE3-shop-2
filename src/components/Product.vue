<template>
    <div class="product-container">
            <div class="img">
                <a :href="href">
                <img :class="{'out-of-stock-img': !stock}" :src="img" alt="socks" />
                <!-- <img :src="require(`../assets/images/${img}`)" :alt="selectedDog"> -->
                </a>
            </div>
            <div class="product-display">
                <h1>{{ title }}</h1>
                <p v-if="stock > 10">Ins Stock</p>
                <h2 v-else-if="stock > 0 && stock <= 10">Almost Sold Out</h2>
                <h2 v-else>Out of stock</h2>
                <h2>Shipping: {{ shipping }}</h2>
                <h2 v-show="onSale">On Sale</h2>
                <p>{{ description }}</p>
                <ul>
                <li v-for="detail in details" :key="detail">{{ detail }}</li>
                </ul>
                <ul>
                <li v-for="size in sizes" :key="size.id">{{ size.size }}</li>
                <li class="color-circle" v-for="(color, index) in colors" :key="color.id" :style="{background: color.color}"
                    @mouseover="updateVariant(index)"></li>
                </ul>
                <Reviews v-for="(review, index) in reviews" :review="review" :key="index" />
                <ReviewForm v-if="showReviewForm" @submit-review="submitReview" />
                <button :disabled="!stock" class="button" :class="{disabledButton: !stock}" @click="addToCart">Buy Now</button>
                <button class="button" @click="removeFromCart">Remove from Cart</button>
                <button class="button" @click="review">Review</button>
            </div>
    </div>
</template>

<script>
import ReviewForm from './ReviewForm.vue';
import Reviews from './Reviews.vue';
export default {
     data() {
        return {
            reviews: [],
            showReviewForm: false,
            product: 'Socks',
            manufacturer: 'David & Co',
            description: 'Description',
            selectedVariant: 0,
            href: 'https:/www.google.com',
            // stock: 5,
            onSale: true,
            details: ['30% cotton', '40% wool', '30% syntetic'],
            sizes: [
                {id:1, size: 'S'},
                {id:2, size: 'M'},
                {id:3, size: 'XL'},
                {id:4, size: 'XXL'},
            ],
            colors: [
                {id:1, color: 'green', img: 'socks_green.jpg', stock: 3},
                {id:3, color: 'blue', img: 'socks_blue.jpg', stock: 2},
            ],
    }},
    props: {
        vipClient: {
            type: Boolean,
            required: false,
        },
        cart: {
            type: Array,
            required: true,
        },
    },
    components: {
        ReviewForm,
        Reviews,
    },
        methods: {
            submitReview(reviews) {
                this.reviews = reviews;
                this.showReviewForm = !this.showReviewForm;
            },
        changeImage(image) {
            this.img = image;
        },
        addToCart() {
            this.$emit('add-to-cart', this.colors[this.selectedVariant].id);
            this.colors[this.selectedVariant].stock -= 1;
        },
        removeFromCart() {
            if (this.cart.length > 0) {
                this.$emit('remove-from-cart', this.colors[this.selectedVariant].id)
                this.colors[this.selectedVariant].stock += 1;
            }
        },
        updateVariant(index) {
            this.selectedVariant = index;
        },
        review() {
            this.showReviewForm = !this.showReviewForm;
        },
    },
    computed: {
        title() {
            const sale = this.onSale? 'On Sale' : '';
            return this.manufacturer + ' ' + this.product + ' ' + sale;
        },
        stock() {
            // console.log('green: ', this.colors[0].stock);
            // console.log('blue: ', this.colors[1].stock);
            return this.colors[this.selectedVariant].stock;
        },
        img() {
            return require(`../assets/images/${this.colors[this.selectedVariant].img}`)
            
        },
        shipping() {
            if (this.vipClient) {
                return 'free';
            } else {
                return 2.99;
            }
        }
    }
}
</script>

<style scoped>
body {
  background-color: #f2f2f2;
  margin: 0px;
  font-family: tahoma;
  color: #282828;
}

.button {
  margin: 30px;
  background-color: #39495c;
  border-radius: 5px;
  font-size: 18px;
  height: 60px;
  color: white;
  padding: 20px;
  box-shadow: inset 0 -0.6em 1em -0.35em rgba(0, 0, 0, 0.17),
    inset 0 0.6em 2em -0.3em rgba(255, 255, 255, 0.15),
    inset 0 0 0em 0.05em rgba(255, 255, 255, 0.12);
  text-align: center;
  cursor: pointer;
}

.color-circle {
  width: 50px;
  height: 50px;
  margin-top: 8px;
  border: 2px solid #d8d8d8;
  border-radius: 50%;
}


.disabledButton {
  background-color: #d8d8d8;
  cursor: not-allowed;
}

h1 {
  font-size: 30px;
  font-weight: bold;
}

h2 {
  font-size: 18px;
}

h3 {
  font-size: 25px;
}

a {
    display: inline-block;
}

img {
  border: 2px solid #d8d8d8;
  width: 90%;
  margin: 40px;
  padding: 15px;
  -webkit-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 2px 15px -12px rgba(0, 0, 0, 0.57);
}

input {
  width: 100%;
  height: 40px;
  margin-bottom: 20px;
}

label {
  font-size: 20px;
  margin-bottom: 5px;
}

li {
    margin-left: 40px;
  font-size: 14px;
  line-height: 16px;
  text-align: left;
}

.nav-bar {
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  height: 60px;
  margin-bottom: 25px;
  -webkit-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.57);
}

.out-of-stock-img {
  opacity:0.5
}

p {
    margin-left: 60px;
    text-align: left;
  font-size: 16px;
}

.img {
    display: inline-block;
    width: 50%;
}
.product-display {
    display: inline-block;
  width: 50%;
}

.product-container {
margin-bottom: 125px;
    align-content: flex-start;
  display: flex;
  flex-direction: row;
}

.product-image,
.product-info {
  width: 50%;
}

.review-form {
  display: flex;
  flex-direction: column;
  width: 425px;
  padding: 20px;
  margin: 40px;
  border: 2px solid #d8d8d8;
  background-color: white;
  -webkit-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 2px 15px -12px rgba(0, 0, 0, 0.57);
}

.review-container {
  width: 425px;
  padding: 20px;
  background-color: white;
  -webkit-box-shadow: 0px 2px 20px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 20px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 2px 20px -12px rgba(0, 0, 0, 0.57);
  margin-left: 40px;
  border: 2px solid #d8d8d8;
}

.review-container li {
  margin-bottom: 30px;
}

.review-form .button {
  display: block;
  margin: 30px auto;
}

select {
  height: 40px;
  font-size: 20px;
  background-color: white;
  cursor: pointer;
}

textarea {
  width: 95%;
  height: 70px;
  padding: 10px;
  font-size: 20px;
  margin-bottom: 20px;
}

ul {
  list-style-type: none;
}

@media only screen and (max-width: 600px) {
  .container {
    flex-direction: column;
  }

  .product-image,
  .product-info {
    margin-left: 10px;
    width: 100%;
  }

  .review-form {
    width: 90%;
  }
}

</style>