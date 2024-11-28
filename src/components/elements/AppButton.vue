<script setup>

</script>

<template>
  <button><slot></slot></button>
</template>

<style scoped>
button {
  font-size: 18px;
  padding: 10px 35px;
  cursor: pointer;
  border-radius: var(--border-radius);
  border: none;
  transition: .4s;
  background-color: var(--white);
  color: var(--black);
}

button:hover {
  color: var(--green-dark);
}
</style>
