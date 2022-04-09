<template>
<div class="row">
    <h3>Name: {{ review.name }}</h3>
    <h3>Rating: {{ review.rate }}</h3>
</div>
<div class="row">
    <p>Comment: {{ review.review }}</p>
</div>
</template>

<script>
export default {
    props: {
        review: {
            type: Object,
            required: true,
        }
    }
}
</script>

<style>
.row {
    height: 30px;
    display: flex;
    justify-content: flex-start;
    border: 1px solid rgb(211, 206, 206);
    width: 90%;
    align-items: center;
    text-align: left;
    margin-left: 60px;
}
h3 {
    margin-left: 50px;
}

p {
    display: block;
    width: 100%;
    margin-left: 50px;
}
</style>