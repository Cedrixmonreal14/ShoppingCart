<template>
  <div class="login-container">
    <img src="../assets/garnierLogo.jpg" alt="Garnier Logo" class="login garnier-logo">
    <!-- <h1 class="login-heading garnier-heading">Login or Register</h1> -->
    <p class="message garnier-message">{{ message }}</p>
    <!-- Login Form -->
    <form v-if="!showRegisterForm" @submit.prevent="login" class="login-form">
      <h1 class="login-heading garnier-heading">Login</h1>
      <label for="loginEmail" class="form-label">Email:</label>
      <input type="email" id="loginEmail" v-model.trim="loginForm.email" required class="input-field">
      <label for="loginPassword" class="form-label">Password:</label>
      <input type="password" id="loginPassword" v-model="loginForm.password" required class="input-field">
      <button type="submit" class="submit-button garnier-button">Login</button>
    </form>

    <!-- Registration Form -->
    <form v-if="showRegisterForm" @submit.prevent="register" class="register-form">
      <h1 class="login-heading garnier-heading">Registration</h1>
      <label for="registerEmail" class="form-label">Email:</label>
      <input type="email" id="registerEmail" v-model.trim="registerForm.email" required class="input-field">
      <label for="registerPassword" class="form-label">Password:</label>
      <input type="password" id="registerPassword" v-model="registerForm.password" required class="input-field">
      <label for="confirmPassword" class="form-label">Confirm Password:</label>
      <input type="password" id="confirmPassword" v-model="confirmPassword" required class="input-field">
      <button type="submit" class="submit-button garnier-button">Register</button>
    </form>

    <!-- Toggle between Login and Registration forms -->
    <button @click="toggleForm" class="toggle-button garnier-button">{{ showRegisterForm ? 'Back to Login' : 'Register' }}</button>

    <!-- Error message -->
    <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
  </div>
</template>

<script>
import { users } from '../data/items.js';

export default {
  
  data() {
    return {
      
      message: '',
      loginForm: {
        email: '',
        password: ''
      },
      registerForm: {
        email: '',
        password: ''
      },
      confirmPassword: '',
      showRegisterForm: false,
      errorMessage: ''
    }
  },
  
  methods: {
    login() {
      const { email, password } = this.loginForm;
      const userFound = users.find(user => user.email === email && user.password === password);
      if (userFound) {
        userFound.isloggedIn = true;
        console.log(userFound.isloggedIn);
        localStorage.setItem('token', '12345');
        this.$router.push({ name: 'home' });
        
      } else {
        this.errorMessage = 'Invalid email or password';
      }
      this.clearFields();
    },

    clearFields() {
      this.loginForm.email = '';
      this.loginForm.password = '';
    },

    register() {
      // Perform registration logic
      if (this.registerForm.password !== this.confirmPassword) {
        this.errorMessage = 'Passwords do not match';
        return;
      }
      // Check if email already exists
      if (users.some(user => user.email === this.registerForm.email)) {
        this.errorMessage = 'Email already registered';
        return;
      }
      
      // Add new user to the users array
      users.push({
        email: this.registerForm.email,
        password: this.registerForm.password, 
        isloggedIn: false,
        userCart: []
      });

      console.log(users);
      this.message = "Registration Complete";
      
      // Reset form fields and error message
      this.registerForm.email = '';
      this.registerForm.password = '';
      this.confirmPassword = '';
      this.errorMessage = '';

      // Redirect to login page after successful registration
      this.showRegisterForm = false;
    },
    toggleForm() {
      this.showRegisterForm = !this.showRegisterForm;
      this.errorMessage = ''; // Clear error message when toggling between forms
    }
  },
  computed: {
    users() {
      // Retrieve users array from wherever it's defined/imported
      return users || [];
    }
  }
}
</script>
