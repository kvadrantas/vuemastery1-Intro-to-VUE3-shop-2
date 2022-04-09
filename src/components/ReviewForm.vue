<template>
<form class="review-content" @submit.prevent="submitReview">
    <h1>Leave your review</h1>
    <label for="name">Your Name</label>
    <input v-model="name" type="text">
    <label for="Your review">Your review</label>
    <textarea v-model="review" name="" id="" cols="30" rows="10"></textarea>
    <label for="rating">Rate</label>
    <select name="" id="" v-model.number="rate">
        <option>1</option>
        <option>3</option>
        <option>5</option>
    </select>
    <button class="button">Submit</button>
</form>

</template>

<script>
export default {
    data() {
        return {
            name: '',
            review: '',
            rate: null,
            reviews: [],
        }
    },
    methods: {
        submitReview() {
            const review = {
                name: this.name,
                review: this.review,
                rate: this.rate,
            }
            this.reviews.push(review);
            // console.log('AAAAAAAA ', this.reviews);
            this.name = '';
            this.review = '';
            this.rate = '';
            this.$emit('submit-review', this.reviews);
        }
    }
}
</script>

<style scoped>
    .review-content {
        align-items: center;
        display: flex;
        flex-direction: column;
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
</style>