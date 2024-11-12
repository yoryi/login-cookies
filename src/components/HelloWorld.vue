<template>
  <div id="app">
    <h1>Login</h1>
    <form @submit.prevent="handleLogin">
      <input
        type="email"
        v-model="email"
        placeholder="Enter your email"
        required
      />
      <input
        type="password"
        v-model="password"
        placeholder="Enter your password"
        required
      />
      <label>
        <input type="checkbox" v-model="rememberMe" />
        Remember Me
      </label>
      <button type="submit">Login</button>
    </form>
    <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      email: "",
      password: "",
      rememberMe: true,
      errorMessage: "",
    };
  },
  methods: {
    async handleLogin() {
      try {
        const response = await axios.post(
          "https://ihs-api-stag-homebuilder-emacbshggrchbkcn.eastus-01.azurewebsites.net/api/users/login",
          {
            email: this.email,
            password: this.password,
            rememberMe: this.rememberMe,
          },
          {
            withCredentials: true,
          }
        );
        console.log("Login successful", response.data);
        console.log("Cookies: ", document.cookie); 
      } catch (error) {
        console.error("Login failed", error);
        this.errorMessage = "Login failed, please try again!";
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

form {
  display: flex;
  flex-direction: column;
  width: 300px;
  margin: 0 auto;
}

input {
  margin: 10px 0;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
}

button {
  padding: 10px;
  font-size: 16px;
  background-color: #2c3e50;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #34495e;
}

.error {
  color: red;
  font-size: 14px;
}
</style>
