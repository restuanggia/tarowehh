<template>
  <div class="card shadow card-product">
    <img
      :src="'assets/images/' + product.gambar"
      class="card-img-top"
      alt="..."
    />
    <div class="card-body">
      <h5>
        <strong class="card-title">{{ product.nama }}</strong>
      </h5>
      <p class="card-text">Harga : Rp. {{ product.harga }}</p>
      <router-link class="btn btn-success" :to="'/foods/' + product.id"
        ><b-icon-cart></b-icon-cart>Pesan</router-link
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: ["product"],
};
</script>

<style scoped>
.card-product .btn-success {
  transition: all 0.3s ease;
}

.card-product .btn-success:hover {
  background-color: #0d47a1; /* biru tua */
  border-color: #0d47a1;
  color: #ffffff;
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(13, 71, 161, 0.4);
}
</style>
