
<template>
  <div>
      <img class="shopLogo garnier-logo" alt="Shop logo" src="../assets/garnierLogo.jpg">
      <h2 class="cart-heading">Cart</h2>
      <p class="message" >{{ cartMessage }}</p>
      <table class="cart-table">
          <thead>
              <tr>
                  <th>Product</th>
                  <th>Price</th>
                  <th>Quantity</th>
                  <th>Action</th>
              </tr>
          </thead>
          <tbody v-for="user in loggedInUsers" :key="user.userId">
            <tr v-for="item in user.userCart" :key="item.id" class="cart-item">
              <td>
                <img :src="item.image" alt="Product Image" width="100" height="100">
                <span class="item-info">{{ item.name }}</span>
              </td>
              <td>₱{{ item.price }}</td>
              <td>
                <input type="number" v-model="item.quantity" @change="updateQuantity(item)" class="quantity-input">
              </td>
              <td>
                <button @click="removeFromCart(item.id)" class="remove-button">Remove</button>
              </td>
            </tr>
          </tbody>
      </table>
      <div >
        <p>Total: ₱{{ totalPrice }}</p>
      </div>
  </div>
</template>

<script>
import { products, cart, users } from '../data/items.js';
export default {
  data() {
      return {
          
          users: users,
          products: products,
          cart: cart,
          cartMessage: ''
      };
  },

  methods: {
    updateQuantity(item) {
        if (item.quantity < 1) {
            // Set the quantity to 1 if it's less than 1
            item.quantity = 1;
        }
        // Update message
        this.cartMessage = `Quantity updated for ${item.name}`;
    },

    removeFromCart(productId) {
        // Find the user who added this product
        const userIndex = this.users.findIndex(user => user.userCart.some(item => item.id === productId));
        if (userIndex !== -1) {
            // Find the index of the product in the user's userCart array
            const userCartIndex = this.users[userIndex].userCart.findIndex(item => item.id === productId);
            if (userCartIndex !== -1) {
                const removedProduct = this.users[userIndex].userCart[userCartIndex].name;
                // Remove the product from the user's userCart array
                this.users[userIndex].userCart.splice(userCartIndex, 1);
                this.cartMessage = `${removedProduct} removed from cart`; // Update message
            }
        }
    },

  },
  computed: {
    totalPrice() {
        // Get the index of the logged-in user
        const loggedInUserIndex = this.users.findIndex(user => user.isloggedIn === true);
        if (loggedInUserIndex !== -1) {
            // Calculate total price for the cart of the logged-in user
            return this.users[loggedInUserIndex].userCart.reduce((total, item) => {
                return total + (item.price * item.quantity);
            }, 0);
        } else {
            return 0;
        }
    },
    loggedInUsers() {
      return this.users.filter(user => user.isloggedIn);
    }
  }
};
</script>
