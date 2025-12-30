<template>
  <div class="keranjang">
    <Navbar :updateKeranjang="keranjangs" />
    <div class="container">
      <!-- Breadcrumb -->
      <div class="row mt-4">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Beranda</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark">Menu</router-link>
              </li>
              <li class="breadcrumb-item active">Keranjang</li>
            </ol>
          </nav>
        </div>
      </div>
      <!-- Tabel Keranjang -->
      <div class="row">
        <div class="col">
          <h2>Keranjang <strong>Saya</strong></h2>

          <div class="table-responsive mt-3">
            <table class="table align-middle">
              <thead class="table-light">
                <tr>
                  <th>
                    <input type="checkbox" @change="toggleAll($event)" />
                  </th>
                  <th>#</th>
                  <th>Foto</th>
                  <th>Makanan</th>
                  <th>Keterangan</th>
                  <th>Jumlah</th>
                  <th>Harga</th>
                  <th>Total</th>
                  <th class="text-center">Hapus</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(keranjang, index) in keranjangs"
                  :key="keranjang.id"
                >
                  <td>
                    <input type="checkbox" v-model="keranjang.dipilih" />
                  </td>
                  <td>{{ index + 1 }}</td>
                  <td>
                    <img
                      :src="'../assets/images/' + keranjang.products.gambar"
                      class="img-fluid rounded shadow-sm"
                      width="120"
                    />
                  </td>
                  <td>
                    <strong>{{ keranjang.products.nama }}</strong>
                  </td>
                  <td>{{ keranjang.keterangan || "-" }}</td>
                  <td>{{ keranjang.jumlah_pemesanan }}</td>
                  <td>Rp. {{ keranjang.products.harga }}</td>
                  <td>
                    <strong>
                      Rp.
                      {{
                        keranjang.products.harga * keranjang.jumlah_pemesanan
                      }}
                    </strong>
                  </td>
                  <td class="text-center">
                    <button
                      class="btn btn-outline-danger btn-sm"
                      @click="hapusKeranjang(keranjang.id)"
                    >
                      <b-icon-trash />
                    </button>
                  </td>
                </tr>
                <!-- Total -->
                <tr>
                  <td colspan="7" class="text-end">
                    <strong>Total Harga :</strong>
                  </td>
                  <td>
                    <strong class="total-harga fs-5">
                      Rp. {{ totalHarga }}
                    </strong>
                  </td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <!-- Form Checkout -->
      <div class="row justify-content-end">
        <div class="col-md-4">
          <form class="mt-4" @submit.prevent="checkout">
            <div class="form-group">
              <label>Nama :</label>
              <input
                type="text"
                class="form-control"
                v-model="pesan.nama"
                placeholder="Masukkan nama"
              />
            </div>

            <button class="btn btn-primary w-100 btn-pesan">
              <b-icon-cart class="me-1" />
              Pesan Sekarang
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "HelloWorld",
  components: { Navbar },

  data() {
    return {
      keranjangs: [],
      pesan: {
        nama: "",
      },
    };
  },

  methods: {
    setKeranjangs(data) {
      this.keranjangs = data.map((item) => ({
        ...item,
        dipilih: true,
      }));
    },

    toggleAll(event) {
      const checked = event.target.checked;
      this.keranjangs.forEach((item) => (item.dipilih = checked));
    },

    hapusKeranjang(id) {
      axios.delete("http://localhost:3000/keranjangs/" + id).then(() => {
        this.keranjangs = this.keranjangs.filter((item) => item.id !== id);
      });
    },

    checkout() {
      const keranjangDipilih = this.keranjangs.filter((item) => item.dipilih);

      if (!this.pesan.nama) {
        this.$toast.error("Nama harus diisi");
        return;
      }

      if (keranjangDipilih.length === 0) {
        this.$toast.error("Pilih minimal 1 produk");
        return;
      }

      const totalHarga = keranjangDipilih.reduce(
        (total, item) => total + item.products.harga * item.jumlah_pemesanan,
        0
      );

      const pesanan = {
        id: Date.now(),
        tanggal: new Date().toLocaleString(),
        nama: this.pesan.nama,
        totalHarga,
        keranjangs: keranjangDipilih,
      };

      let riwayat = localStorage.getItem("riwayatPesanan");
      riwayat = riwayat ? JSON.parse(riwayat) : [];
      riwayat.push(pesanan);
      localStorage.setItem("riwayatPesanan", JSON.stringify(riwayat));

      axios.post("http://localhost:3000/pesanans", pesanan).then(() => {
        // Hapus hanya item yang dipilih
        keranjangDipilih.forEach((item) => {
          axios.delete("http://localhost:3000/keranjangs/" + item.id);
        });

        this.$router.push("/pesanan-sukses");
      });
    },
  },

  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((res) => this.setKeranjangs(res.data));
  },

  computed: {
    totalHarga() {
      return this.keranjangs
        .filter((item) => item.dipilih)
        .reduce(
          (total, item) => total + item.products.harga * item.jumlah_pemesanan,
          0
        );
    },
  },
};
</script>

<style scoped>
table tbody tr:hover {
  background-color: #f8f9fa;
}

.btn-pesan {
  background-color: #1e88e5;
  border: none;
  font-weight: 600;
  padding: 12px;
}

.btn-pesan:hover {
  background-color: #0d47a1;
}

.total-harga {
  color: #1e88e5;
  font-weight: 700;
}

.total-harga:hover {
  color: #0d47a1;
}
</style>
