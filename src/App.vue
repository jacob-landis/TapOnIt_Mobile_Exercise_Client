<template>
  <div id="app">
    <Products v-bind:products='products' 
      v-on:toggle-liked='toggleLike'
      v-on:product-clicked='displayProductDetails' />
    <ProductDetails v-bind:detailsModal='detailsModal'
      v-on:toggle-liked='toggleLike' />
  </div>
</template>

<script>
import Products from './components/Products';
import ProductDetails from './components/ProductDetails';
import axios from 'axios';

const url = 'http://localhost:5000/api/posts';

export default {
  name: 'app',
  components: {
    Products,
    ProductDetails
  },
  data() {
    return {
      products: [],
      detailsModal: {
        display: false,
        // unlike the Products component, the ProductDetails component needs placeholders
        product: {id:-1,title:'',decription:'',image:'Default.jpg',price:0,likes:0}
      }
    }
  },
  created(){
    // get products from server
    axios.get(url)
      .then(res => {
        this.products = res.data.recordset;
        this.detailsModal.product = this.products[0];
      })
      .catch(err => console.log(err));
  },
  methods: {
    displayProductDetails(id) {
      this.products.forEach(p => { 
        if (p.id == id) {
          this.detailsModal.product = p;
          this.detailsModal.display = true;
        }
      });
    },
    hideProductDetails(){
      this.detailsModal.display = false;
    },
    toggleLike(id) {
      // get handle on product with id param
      this.products.forEach(p => { 
        if (p.id == id) {

          // toggle userLiked flag
          p.userLiked = !p.userLiked;

          // incr/decr like count (on client)
          p.likes += p.userLiked ? 1 : -1;
          
          // post id of product to be incr/decr (represented by bool value)
          axios.post(url, { id: id, value: p.userLiked });
        }
      });
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
</style>
