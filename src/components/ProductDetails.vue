<template>
    <div id='modal' ref='modal' class='modalBackground' v-bind:class="{'displayed-modal': detailsModal.display}">
        <div id='productDetails' v-bind='detailsModal'>
            <img ref='prodImg' class='prodImg' />
            <h3>{{detailsModal.product.title}}</h3>
            <h5>${{detailsModal.product.price}}</h5>
            <p>{{detailsModal.product.description}}</p>
            <button v-on:click="$emit('toggle-liked', detailsModal.product.id)" 
                v-bind:class="{'user-liked': detailsModal.product.userLiked}">Like</button>
            <h4>likes {{detailsModal.product.likes}}</h4>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ProductDetails',
    props: ['detailsModal'],
    mounted: function() {
        // keeps modal in viewport
        window.onscroll =()=> this.$refs.modal.style.top = `${window.scrollY}px`
        // close modal
        this.$refs.modal.onclick =e=> { if (e.target.id == 'modal') this.detailsModal.display = false }

        // work-around for updating prodImg src
        // triggered when displayed-modal class is toggled
        var observer = new MutationObserver(()=> {
            this.$refs.prodImg.src = require('@/assets/' + this.detailsModal.product.image);
        });
        observer.observe(this.$refs.modal, { attributes: true });
    }
}
</script>

<style scoped>
    #productDetails {
        height: 500px;
        border: solid black 1px;
        margin: 200px 25% 0 25%;
        padding: 40px;
    }
    .modalBackground {
        display: none; /* Hidden by default */
        position: absolute;
        top: 40px;
        width: 100%;
        height: 100%;
        background-color: rgba(255, 255, 255, 0.5);
    }
    .displayed-modal {
        display: block;
    }
    button {
        border: none;
        outline: none;
        padding: 5px;
    }
    .user-liked {
        background-color:cornflowerblue;
    }
    h4 {
        display: inline;
        padding-left: 5px;
    }
</style>