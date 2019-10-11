<template>
    <div class='product'>
        <img ref='prodImg' class='prodImg' v-on:click="$emit('product-clicked')" />
        <h3 v-on:click="$emit('product-clicked')">{{product.title}}</h3>
        <h5>${{product.price}}</h5>
        <p>{{product.description}}</p>
        <button v-on:click="$emit('toggle-liked')" v-bind:class="{'user-liked': product.userLiked}">Like</button>
        <h4>likes {{product.likes}}</h4>
    </div>
</template>

<script>
export default {
    name: 'Product',
    props: ['product'],
    // sets image (could not get v-bind:src to work)
    mounted: function() { this.$refs.prodImg.src = require('@/assets/' + this.product.image); }
}
</script>

<style scoped>
    .prodImg {
        max-width: 100px;
        max-height: 100px;
        cursor: pointer;
    }
    .product {
        margin: 40px;
    }
    .user-liked {
        background-color:cornflowerblue;
    }
    button {
        border: none;
        outline: none;
        padding: 5px;
    }
    h3 {
        cursor: pointer;
    }
    h4 {
        display: inline;
        padding-left: 5px;
    }
</style>