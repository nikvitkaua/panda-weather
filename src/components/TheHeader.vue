<script setup>
</script>

<template>
  <header class="header">
    <h1 class="logo">Panda Weather</h1>
    <nav>
      <ul class="header__menu">
        <li><a class="header__link" href="/">Home</a></li>
        <li><a class="header__link" href="/favorites">Favorites</a></li>
      </ul>
    </nav>
  </header>
</template>

<style scoped>
.header {
  padding: 15px;
  margin-top: 10px;
  border-radius: var(--border-radius);
  background: var(--black);
  color: var(--white)
}

.header .logo {
  text-align: center;
  font-size: 32px;
  margin-bottom: 20px;
}

.header__menu {
  display: flex;
  align-items: center;
  justify-content: start;
  gap: 25px;
}

.header__link {
  font-size: 20px;
}

.header__link:hover {
  color: var(--green-dark);
}
</style>
