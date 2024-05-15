<template>
  <button>
    <slot></slot>
  </button>
</template>

<style scoped>
button {
  border-radius: 10px;
  border: none;
  padding: 10px 20px;
  color: white;
  background-color: rgb(60, 62, 68);
}

button:disabled {
  border-radius: 10px;
  border: none;
  padding: 10px 20px;
  color: rgb(69, 68, 68);
  background-color: rgb(53, 57, 67);
}

button:hover {
  background: #222222;
  box-shadow: none;
  position: relative;
  transition: all 0.25s ease-in-out;
}
</style>
