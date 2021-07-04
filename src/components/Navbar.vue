<template>
  <div class="navbar">
    <div @click="$router.push('/')">
      Post Application
    </div>
    <div class="navbar__btns">
      <my-button @click="$router.push('/posts')" class="nav__btn">Посты</my-button>
      <my-button @click="$router.push('/store')" class="nav__btn">Store</my-button>
      <my-button @click="$router.push('/composition')" class="nav__btn">Composition</my-button>
      <my-button @click="$router.push('/about')" class="nav__btn">О сайте</my-button>
    </div>
  </div>
</template>

<script>

export default {
  name: "my-navbar"
}
</script>

<style scoped>
.navbar {
  height: 50px;
  background-color: lightgray;
  box-shadow: 2px 2px 4px gray;
  display: flex;
  align-items: center;
  padding: 0 15px;
}
.navbar__btns {
  margin-left: auto;
}
.nav__btn {
  margin-left: 20px;
}
</style>