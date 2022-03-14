<template>
  <div class="product">
    <div class="product-image">
      <img v-bind:src="image" alt="" />
    </div>

    <div class="product-info">
      <h1>{{ title }}</h1>
      <p>{{ description }}</p>
      <p v-if="inStock">In Stock</p>
      <p v-else>Out of Stock</p>

      <!-- <p v-if="inventory > 10">In stock</p>
            <p v-else-if="inventory <= 10 && inventory > 0">Almost sold out!</p>
            <p v-else>Out of stock</p> -->
      <p>Shipping {{ shipping }}</p>

      <!-- to forskellige metoder: -->
      <p>{{ sale }}</p>
      <!-- <span v-if="onSale">On Sale!</span> -->

      <ProductDetails :details="details"></ProductDetails>

      <!-- <ul>
                <li v-for="size in sizes">{{ size }}</li>
            </ul> -->

      <div class="flex-box">
        <div
          class="color-box"
          v-for="(variant, index) in variants"
          :key="variant.variantId"
          :style="{ backgroundColor: variant.variantColor }"
          @click="updateProduct(index)"
        ></div>
      </div>

      <div>
        <button
          v-on:click="addToCart"
          :disabled="!inStock"
          :class="{ disabledButton: !inStock }"
        >
          Add to cart
        </button>
        <button @click="removeFromCart">Remove from cart</button>
      </div>
    </div>

    <div>
      <h2>Reviews</h2>
      <p v-if="!reviews.length">There are no reviews yet.</p>
      <ul v-else>
        <li v-for="(review, index) in reviews" :key="index">
          <p>{{ review.name }}</p>
          <p>Rating:{{ review.rating }}</p>
          <p>{{ review.review }}</p>
        </li>
      </ul>
    </div>

    <!-- <ProductReview @review-submitted="addReview"></ProductReview> -->
  </div>
</template>

<script>
import ProductDetails from "./ProductDetails.vue";
// import ProductReview from './ProductReview.vue';
// import { eventBus } from '../main.js';

export default {
  name: "Product",
  components: {
    ProductDetails,
    // ProductReview
  },
  props: {
    premium: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      product: "Socks",
      brand: "NoName",
      description: "A pair of warm socks",
      selectedVariant: 0,
      // inventory: 8,
      details: ["80% cotton", "20% polyester", "Gender-neutral"],
      // sizes: ["36", "38", "40", "42", "44", "46"],
      variants: [
        {
          variantId: 22,
          variantColor: "green",
          variantImage: require("../assets/images/vmSocks-green.jpg"),
          variantQuantity: 10,
        },
        {
          variantId: 35,
          variantColor: "blue",
          variantImage: require("../assets/images/vmSocks-blue.jpg"),
          variantQuantity: 0,
        },
      ],
      cart: 0,
      onSale: true,
      reviews: [],
    };
  },
  methods: {
    addToCart() {
      this.$emit("add-to-cart", this.variants[this.selectedVariant].variantId);
    },
    updateProduct(index) {
      this.selectedVariant = index;
    },
    removeFromCart() {
      this.$emit(
        "remove-from-cart",
        this.variants[this.selectedVariant].variantId
      );
    },
    addReview(ProductReview) {
      this.reviews.push(ProductReview);
    },
  },
  computed: {
    title() {
      return this.brand + " " + this.product;
    },
    image() {
      return this.variants[this.selectedVariant].variantImage;
    },
    inStock() {
      return this.variants[this.selectedVariant].variantQuantity;
    },
    sale() {
      if (this.onSale) {
        return this.brand + " " + this.product + " are on sale!";
      }
      return this.brand + " " + this.product + " are not on sale";
    },
    shipping() {
      if (this.premium) {
        return "Free";
      }
      return 2.99;
    },
  },
  // mounted() {
  //   eventBus.$on('review-submitted', productReview => {
  //     this.addReview.push(productReview)
  //   })
  // }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
