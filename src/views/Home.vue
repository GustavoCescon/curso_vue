<template>
  <Banner/>
  <div class="mainContainer">
    <h1>Monte seu burguer:</h1>
    <BurgerForms/>
  </div>
</template>

<script>
  import Banner from "../components/Banner.vue"
  import BurgerForms from "../components/BurgerForms.vue"
  export default{
    name: "Home",
    components: {
      Banner,
      BurgerForms
    }
  }
</script>
