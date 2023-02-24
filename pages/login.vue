<template>
  <v-app>
    <v-main>
      <v-card>
        <h1>Log in</h1>
        <v-form @submit.prevent="login">
          <label class="label" for="email">Email:</label>
          <v-text-field id="email" v-model="email" type="email" name="email" />
          <div v-if="emailError">{{ emailError }}</div>
          <br /><br />
          <label class="label" for="password">Password:</label>
          <v-text-field
            id="password"
            v-model="password"
            type="password"
            name="password"
          />
          <div v-if="passwordError">{{ passwordError }}</div>
          <br /><br />
          <v-btn type="submit" :disabled="isDisabled">log in</v-btn>
          <v-btn flat to="/register" nuxt>create new account</v-btn>
          <div v-if="loginError">{{ loginError }}</div>
        </v-form>
      </v-card>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      email: "",
      password: "",
      emailError: "",
      passwordError: "",
      loginError: "",
      isDisabled: true,
    };
  },
  watch: {
    email: ["validateEmail", "validateForm"],
    password: ["validatePassword", "validateForm"],
  },
  methods: {
    validateEmail() {
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      if (!this.email) {
        this.emailError = "Please, type your e-mail";
      } else {
        this.emailError = "";
      }
    },

    validatePassword() {
      if (!this.password) {
        this.passwordError = "Please enter your password";
      } else {
        this.passwordError = "";
      }
    },

    validateForm() {
      if (
        this.email &&
        this.password &&
        !this.passwordError &&
        !this.emailError
      ) {
        this.isDisabled = false;
      } else {
        this.isDisabled = true;
      }
    },

    login() {
      const users = JSON.parse(localStorage.getItem("user"));
      const user = users.find(
        (user) => user.email === this.email && user.password === this.password
      );
      if (user) navigateTo("/");
      else this.loginError = "Wrong email or password";
    },
  },
};
</script>

<style></style>
