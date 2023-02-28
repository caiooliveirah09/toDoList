<template>
  <div>
    <NuxtPage />
  </div>
</template>

<style>
.div-app {
  position: relative;
  height: 100vh;
  background-color: #f5f5f5;
}

.div-card {
  width: 100%;
  max-width: 550px;
  height: 100%;
  max-height: 600px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  flex-direction: column;
}

.div-v-btn {
  margin: 1%;
}

.v-text-field {
  width: 100%;
}

.v-form {
  width: 100%;
}

.v-btn {
  background-color: #5e9ce5;
  margin: 5px;
}

.todo-card {
  width: 100%;
}

.v-col {
  display: flex;
  padding: 10px 0;
  background-color: white;
}

.v-row {
  margin: 0;
}

.v-container {
  padding: 0;
  width: 100%;
}

.v-text-field-index {
  width: 100%;
  height: 100%;
}
</style>
