<template>
  <div>
    <b-navbar toggleable="lg" type="bold">
      <div class="container mt-3">
        <b-navbar-brand href="#"><strong>Tarowehh</strong></b-navbar-brand>
        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <li class="nav-item">
              <router-link class="nav-link" to="/">Beranda</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/foods">Menu</router-link>
            </li>
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <li class="nav-item">
              <router-link class="nav-link" to="/keranjang">
                <!-- Keranjang -->
                <b-icon-cart></b-icon-cart>
                <span class="badge badge-success ml-2">{{
                  updateKeranjang
                    ? updateKeranjang.length
                    : jumlah_pesanans.length
                }}</span>
              </router-link>
            </li>
          </b-navbar-nav>
          <b-navbar-nav>
            <li class="nav-item">
              <router-link class="nav-link" to="/riwayat">
                Riwayat Transaksi
                <!-- <b-icon-bag></b-icon-bag>
                  <span
                    class="badge badge-success ml-2"
                  >{{ updateKeranjang ? updateKeranjang.length : jumlah_pesanans.length }}</span> -->
              </router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/stok-barang"
                >Stok Barang</router-link
              >
            </li>
          </b-navbar-nav>
        </b-collapse>
      </div>
    </b-navbar>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  data() {
    return {
      jumlah_pesanans: [],
    };
  },
  props: ["updateKeranjang"],
  methods: {
    setJumlah(data) {
      this.jumlah_pesanans = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => this.setJumlah(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style></style>
