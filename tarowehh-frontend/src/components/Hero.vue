<template>
  <div class="hero">
    <div class="row mt-4 align-items-center">
      <!-- Text -->
      <div class="col-md-6 order-2 order-md-1">
        <h2>
          <strong>Tarowehh Chips Menu,</strong><br />
          in Your Snacking
        </h2>
        <p>
          Keripik Klasik yang Berbeda dengan Keunikan talas dalam bentuk keripik
          yang tiada duanya.
        </p>
        <router-link class="btn btn-lg btn-success" to="/foods">
          <b-icon-arrow-right /> Pesan
        </router-link>
      </div>

      <!-- Image -->
      <div class="col-md-6 order-1 order-md-2 mb-3 mb-md-0">
        <img
          src="../assets/images/Hero.png"
          class="img-fluid"
          alt="Hero Tarowehh"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
};
</script>

<style scoped>
.hero {
  min-height: 80vh;
}
</style>
