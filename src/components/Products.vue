<template>
    <div>
        <div v-bind:key='product.id' v-for='product in products'>
            <Product v-bind:product='product' 
                v-on:toggle-liked="$emit('toggle-liked', product.id)"
                v-on:product-clicked="$emit('product-clicked', product.id)" />
        </div>
    </div>
</template>

<script>
import Product from './Product';

export default {
    name: 'Products',
    components: {Product},
    props: ['products']
}
</script>