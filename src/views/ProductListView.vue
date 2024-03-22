
<template>
  <div>
      <img class="shopLogo garnier-logo" src="../assets/garnierLogo.jpg">
      <h1 class="product-heading garnier-heading">Products</h1>
      <p class="message garnier-message">{{ productMessage }}</p>
      <div>
          <table class="garnier-table">
              <thead>
                  <tr>
                      <th>Product Image</th>
                      <th>Product Name</th>
                      <th>Price</th>
                      <th>Action</th>
                  </tr>
              </thead>
              <tbody>
                  <tr v-for="product in products" :key="product.id">
                      <td><img :src="product.image" alt="Product Image" width="100" height="100"></td>
                      <td>{{ product.name }}</td>
                      <td>₱{{ product.price }}</td>
                      <td><button @click="addToCart(product)" class="add-to-cart-btn">Add to Cart</button></td>
                  </tr>
              </tbody>
          </table>
      </div>
      <center>
        <button @click="handleLogout" class="logout-button">Logout</button>
      </center>
  </div>
</template>

<script>
import { products, cart, users } from '../data/items.js';

export default {
  data(){
    
      return {
          users: users,
          products: products,
          cart: cart,
          productMessage: '',
          loggedInUserEmail: ''
          
      };
  },
methods: {
    handleLogout() {
        localStorage.removeItem('token');
        this.$router.push('/login');
        const userIndex = this.users.findIndex(user => user.isloggedIn === true); 
        this.users[userIndex].isloggedIn = false;
        console.log(this.users[userIndex].isloggedIn = false)
        
    },

    addToCart(product) {
        const loggedInUserIndex = this.users.findIndex(user => user.isloggedIn === true);
        console.log(users)
        const userCart = this.users[loggedInUserIndex].userCart;
        const index = userCart.findIndex(item => item.id === product.id);
            if (index !== -1) {
                userCart[index].quantity++;
            } else {
                userCart.push({ ...product, quantity: 1 });
                // console.log(users)
                // this.productMessage = `${product.name} added to cart`; // Update message
            }
            console.log(this.users);
            this.productMessage = `${product.name} added to cart`;
    },
  }
}
</script>
