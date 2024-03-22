<template>
    <div class="ShoppingCart">
      <!-- aayusin -->
      <h1>Shopping Cart</h1>
      <div v-for="(item, index) in cart" :key="index" class="cart-item">
        <div class="item-details">
          <p class="item-name">{{ item.name }}</p>
          <p class="item-price">₱ {{ item.price }}</p>
          <p class="item-quantity">Quantity: {{ item.quantity }}</p>
        </div>
        <div class="buttons">
          <button @click="openDialog(index)" class="update-btn">Update</button>
          <button @click="removeFromCart(index)" class="remove-btn">Remove</button>
        </div>
      </div>
  
  
      <p class="total">Total: ₱ {{ total }}</p>
  

      <div v-if="dialogVisible" class="dialog">
        <div class="dialog-content">
          <p>Enter new quantity for {{ cart[selectedItemIndex].name }}:</p>
          <input type="number" v-model="newQuantity" @keydown.enter="updateQuantity(selectedItemIndex)">
          <button @click="updateQuantity(selectedItemIndex)">Save</button>
          <button @click="closeDialog">Cancel</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
 export default {
  data: () => ({
    dialogVisible: false,
    selectedItemIndex: null,
    newQuantity: '', 
  }),
  props: {
    cart: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    total() {
      return this.cart.reduce((acc, item) => acc + item.price * item.quantity, 0);
    },
  },
  methods: {
    openDialog(index) {
      this.selectedItemIndex = index;
      this.newQuantity = this.cart[index].quantity.toString();
      this.dialogVisible = true;
    },
    closeDialog() {
      this.dialogVisible = false;
    },
    updateQuantity(index) {
      const newQuantity = parseInt(this.newQuantity);
      if (!isNaN(newQuantity) && newQuantity >= 0) {
        this.$emit('update-quantity', { index, quantity: newQuantity });
        this.closeDialog();
      } else {
        alert('Please enter a valid number');
      }
    },
    removeFromCart(index) {
      this.$emit('remove-from-cart', index);
    },
  },
};

  </script>
  


  <style>
  .ShoppingCart {
    margin-top: 20px;
    position: absolute;
    right: 30%;
  }
  
  .cart-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    border-bottom: 1px solid #ccc;
  }
  
  .item-details {
    flex: 1;
  }
  
  .item-name {
    margin: 0;
    font-weight: bold;
  }
  
  .item-quantity {
    margin: 0;
  }
  
  .item-price {
    margin: 0;
  }
  
  .buttons {
    display: flex;
    align-items: center;
  }
  
  button {
    margin-left: 10px;
  }
  
  .total {
    font-size: 18px;
    font-weight: bold;
    margin-top: 10px;
  }
  
  .dialog {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .dialog-content {
    background-color: white;
    padding: 20px;
    border-radius: 5px;
  }
  
  .dialog-content input {
    width: 100px;
  }



  
.update-btn {
  background-color: #0015ff;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
  width: 100px;
} 

.update-btn:hover{
  background-color: #000fae;
}





.remove-btn {
  background-color: #ff0000;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
  width: 100px;
} 

.remove-btn:hover{
  background-color: #cc0202;
}


  </style>
