<template>
  <div class="riwayat">
    <Navbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <h2>Riwayat <strong>Pesanan</strong></h2>
          <div v-if="riwayatPesanan.length === 0" class="alert alert-info mt-3">
            Belum ada pesanan yang berhasil dilakukan.
          </div>
          <div class="table-responsive mt-3" v-else>
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Tanggal</th>
                  <th scope="col">Nama</th>
                  <th scope="col">Total Harga</th>
                  <th scope="col">Detail</th>
                  <th scope="col" class="text-center">Hapus</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, index) in riwayatPesanan" :key="index">
                  <th>{{ index + 1 }}</th>
                  <td>{{ item.tanggal }}</td>
                  <td>{{ item.nama }}</td>
                  <td>Rp. {{ item.totalHarga }}</td>
                  <td>
                    <button
                      class="btn btn-detail btn-sm"
                      @click="lihatDetail(item.id)"
                    >
                      <b-icon-eye class="mr-1" />
                      Detail
                    </button>
                  </td>
                  <td class="text-center">
                    <button
                      class="btn btn-outline-danger btn-sm btn-icon"
                      @click="hapusRiwayatPesanan(item.id)"
                      title="Hapus"
                    >
                      <b-icon-trash />
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";

export default {
  name: "RiwayatPesanan",
  components: {
    Navbar,
  },
  data() {
    return {
      riwayatPesanan: [],
    };
  },
  methods: {
    lihatDetail(id) {
      this.$router.push({ path: `/detail-pesanan/${id}` });
    },
    loadRiwayatPesanan() {
      const riwayat = localStorage.getItem("riwayatPesanan");
      if (riwayat) {
        this.riwayatPesanan = JSON.parse(riwayat);
      }
    },
    hapusRiwayatPesanan(id) {
      // Saring item untuk menghapus secara lokal
      this.riwayatPesanan = this.riwayatPesanan.filter(
        (item) => item.id !== id
      );
      localStorage.setItem(
        "riwayatPesanan",
        JSON.stringify(this.riwayatPesanan)
      );
      this.$toast.error("Sukses Hapus Riwayat Pesanan", {
        type: "error",
        position: "top-right",
        duration: 3000,
        dismissible: true,
      });
    },
  },
  mounted() {
    this.loadRiwayatPesanan();
  },
};
</script>

<style scoped>
.btn-detail {
  background-color: #1e88e5; /* biru utama */
  color: #ffffff;
  border: none;
  transition: all 0.3s ease;
}

.btn-detail:hover {
  background-color: #0d47a1; /* biru tua */
  color: #ffffff;
}

.aksi {
  display: flex;
  justify-content: center;
}

.btn-icon {
  padding: 6px 8px;
  line-height: 1;
}
</style>
