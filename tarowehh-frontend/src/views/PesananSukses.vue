<template>
  <div class="pesanan-sukses">
    <Navbar />
    <div class="container">
      <div class="row justify-content-center mt-5">
        <div class="col text-center">
          <img src="../assets/images/menunggu.png" width="300" class="mb-4" />
          <h2 class="text-success">
            <strong>Sukses!!</strong>
          </h2>
          <h4 class="text-muted">
            Pesanan Anda segera diproses <br />
            Selamat Menunggu
          </h4>
          <p class="mt-3 text-secondary">
            Anda akan diarahkan kembali ke halaman utama...
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";

export default {
  name: "PesananSukses",
  components: {
    Navbar,
  },
  mounted() {
    setTimeout(() => {
      this.$router.push("/");
    }, 5000);
  },
};
</script>

<style scoped>
.pesanan-sukses img {
  animation: fadeIn 0.8s ease-in-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
