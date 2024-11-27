<script setup>

</script>

<template>
  <div class="search">
    <input type="text" placeholder="Find your city...">
    <button>Add</button>
  </div>
</template>

<style scoped>
.search {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 40px;
  width: 100%;
  background: var(--black);
  padding: 15px;
  margin-top: 20px;
  border-radius: var(--border-radius);
}

.search input {
  height: 20px;
  width: 100%;
  border-radius: var(--border-radius);
  padding: 20px;
  background: var(--black);
  outline: none;
  border: 2px solid var(--white);
  color: var(--white);
  font-size: 16px;
  font-family: var(--font);
}

.search input:focus {
  border: 2px solid var(--green-dark);
}

.search button {
  font-size: 18px;
  padding: 10px 25px;
  cursor: pointer;
  border-radius: var(--border-radius);
  border: none;
  transition: .4s;
  background-color: var(--white);
  color: var(--black);
}

.search button:hover {
  color: var(--green-dark);
}
</style>
