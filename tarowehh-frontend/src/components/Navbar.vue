<template>
  <b-navbar toggleable="lg" class="navbar-custom">
    <div class="container mt-3">
      <b-navbar-brand href="/"><strong>Tarowehh</strong></b-navbar-brand>
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

        <b-navbar-nav class="ml-auto">
          <li class="nav-item">
            <router-link class="nav-link" to="/keranjang">
              <b-icon-cart />
              <span class="badge badge-success ml-2">
                {{
                  updateKeranjang
                    ? updateKeranjang.length
                    : jumlah_pesanans.length
                }}
              </span>
            </router-link>
          </li>
        </b-navbar-nav>

        <b-navbar-nav>
          <li class="nav-item">
            <router-link class="nav-link" to="/riwayat">
              Riwayat Transaksi
            </router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/stok-barang">
              Stok Barang
            </router-link>
          </li>
        </b-navbar-nav>
      </b-collapse>
    </div>
  </b-navbar>
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

<style scoped>
/* Navbar dasar */
.navbar-custom {
  position: sticky;
  top: 0;
  z-index: 1000;
  background-color: #ffffff;
  border-bottom: 1px solid rgba(30, 136, 229, 0.25);
}

/* Link navbar */
.navbar-custom .nav-link {
  color: #1e88e5;
  font-weight: 500;
  transition: color 0.3s ease;
}

/* Hover link */
.navbar-custom .nav-link:hover {
  color: #0d47a1;
}

/* Brand */
.navbar-custom .navbar-brand {
  color: #1e88e5;
}

.navbar-custom .navbar-brand:hover {
  color: #0d47a1;
}

/* Badge keranjang */
.navbar-custom .badge-success {
  background-color: #42a5f5;
}

/* Optional: biar badge lebih soft */
.navbar-custom .badge {
  font-weight: 600;
}

/* Link navbar aktif (Beranda / Menu) */
.navbar-custom .router-link-exact-active {
  color: #0d47a1; /* biru lebih gelap */
  font-weight: 700; /* bold */
}

.navbar-custom.navbar-light .nav-link {
  color: #1e88e5 !important;
}

.navbar-custom.navbar-light .nav-link:hover {
  color: #0d47a1 !important;
}
</style>
