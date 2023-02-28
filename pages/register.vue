<template>
  <v-app class="div-app">
    <v-card class="div-card">
      <h1>Create a new account!</h1>
      <h2>is fast.</h2>
      <v-form @submit.prevent="register">
        <v-text-field
          id="name"
          v-model="name"
          type="name"
          name="name"
          label="Name"
        />
        <div v-if="nameError">{{ nameError }}</div>
        <v-text-field
          id="email"
          v-model="email"
          type="email"
          name="email"
          label="Email"
        />
        <div v-if="emailError">{{ emailError }}</div>
        <v-text-field
          id="password"
          v-model="password"
          type="password"
          name="password"
          label="Password"
        />
        <div class="div-v-btn" v-if="passwordError">{{ passwordError }}</div>
        <v-btn type="submit" :disabled="isDisabled">sign up</v-btn>
        <v-btn flat to="/login" nuxt>i already have an account</v-btn>
      </v-form>
    </v-card>
  </v-app>
</template>

<script>
export default {
  name: "index",
  mounted() {
    const user = JSON.parse(localStorage.getItem("user"));
    if (user) navigateTo("/");
  },
  data() {
    return {
      name: "",
      email: "",
      password: "",
      nameError: "",
      emailError: "",
      passwordError: "",
      isDisabled: true,
    };
  },
  watch: {
    name: ["validateName", "validateForm"],
    email: ["validateEmail", "validateForm"],
    password: ["validatePassword", "validateForm"],
  },
  methods: {
    validateName() {
      if (!this.name) {
        this.nameError = "Please, type your name";
      } else {
        this.nameError = "";
      }
    },

    validateEmail() {
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      if (!this.email) {
        this.emailError = "Please, type your e-mail";
      } else if (!emailRegex.test(this.email)) {
        this.emailError = "Please, type a valid email";
      } else {
        this.emailError = "";
      }
    },

    validatePassword() {
      const lowerCase = /.*[a-z]+.*/;
      const upperCase = /.*[A-Z].*/;
      const numberRegex = /[0-9]/;
      const specialCharacterRegex = /[^a-zA-Z 0-9]+/g;

      if (!this.password) {
        this.passwordError = "Please enter your password";
      } else if (this.password.length < 6) {
        this.passwordError = "Password must be at least 6 characters long";
      } else if (!lowerCase.test(this.password)) {
        this.passwordError = "Password must have at least one lowercase letter";
      } else if (!upperCase.test(this.password)) {
        this.passwordError = "Password must have at least one capital letter";
      } else if (!numberRegex.test(this.password)) {
        this.passwordError = "Password must have at least one number";
      } else if (!specialCharacterRegex.test(this.password)) {
        this.passwordError =
          "Password must have at least one special character ($,#,@,!,etc)";
      } else {
        this.passwordError = "";
      }
    },

    validateForm() {
      if (
        this.name &&
        this.email &&
        this.password &&
        !this.nameError &&
        !this.passwordError &&
        !this.emailError
      ) {
        this.isDisabled = false;
      } else {
        this.isDisabled = true;
      }
    },

    register() {
      const users = JSON.parse(localStorage.getItem("users"));
      const newUser = {
        name: this.name,
        email: this.email,
        password: this.password,
        tasks: [],
      };
      if (!users) {
        localStorage.setItem("users", JSON.stringify([{ id: 0, ...newUser }]));
        localStorage.setItem("user", JSON.stringify({ id: 0, ...newUser }));
      } else {
        users.push({ id: users.length, ...newUser });
        localStorage.setItem("users", JSON.stringify(users));
        localStorage.setItem(
          "user",
          JSON.stringify({ id: users.length - 1, ...newUser })
        );
      }
      console.log("successfully registered user!", this.email, this.password);
      navigateTo("/");
    },
  },
};
</script>

<style></style>
