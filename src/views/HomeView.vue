<script>
import Banner from '../components/Banner.vue'
import BurgerForm from '../components/BurgerForm.vue'

export default{
  components:{
    Banner,
    BurgerForm
  }
}
</script>

<template>
  <div class="main-container">
    <Banner />
    <h1>Monte o seu Burger</h1>
    <BurgerForm />
  </div>
</template>
<style>
h1{
  font-weight: 800;
  margin-top: 30px;
}
</style>