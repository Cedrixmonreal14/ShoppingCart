<template>


  
  <div class="item-list">
    <h1>Product List</h1>
    <div v-for="(item, index) in items" :key="index" class="item">
      <div class="item-details">
        <p class="item-name">{{ item.name }}</p>
        <p class="item-price">₱ {{ item.price }}</p>
      </div>
      <div class="buttons">
        <button @click="addToCart(item)" class="add-to-cart-btn">Add to Cart</button>
        <button @click="updateItem(index)" class="update-btn">Update</button>
        <button @click="removeItem(index)" class="remove-btn">Remove</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [
        { name: 'Product 1', price: 100 },
        { name: 'Product 2', price: 100 },
        { name: 'Product 3', price: 100 },
        { name: 'Product 4', price: 100 },
        { name: 'Product 5', price: 100 },
        { name: 'Product 6', price: 100 }
      ]
    };
  },
  methods: {
    addToCart(item) {
      this.$emit('add-to-cart', item);
    },
    updateItem(index) {
      const item = this.items[index];
      const newName = prompt('Enter the new name:', item.name);
      const newPrice = parseFloat(prompt('Enter the new price:', item.price));
      
      if (newName && !isNaN(newPrice)) {
        item.name = newName;
        item.price = newPrice;
      }
    },
    removeItem(index) {
      this.items.splice(index, 1);
    }
  }
};
</script>

<style>
.item-list {
  
  display: absolute;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
  margin-left: 0px;
}

.item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ccc;
  width: 80%;
}

.item-details {
  flex: 1;
}

.item-name {
  margin: 0;
  width: 100px;
  font-weight: bold;
}



.buttons {
  display: flex;
  gap: 10px;
}




.add-to-cart-btn {
  background-color: #ff7700;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
  width: 100px;
} 

.add-to-cart-btn:hover{
  background-color: #c15c03;
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
