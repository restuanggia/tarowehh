<template>
  <div class="stok-barang">
    <Navbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <h2>Manajemen <strong>Stok Barang</strong></h2>

          <!-- Filter berdasarkan tanggal -->
          <div class="mt-3">
            <label for="filter-date">Filter Tanggal:</label>
            <input
              type="date"
              id="filter-date"
              v-model="filterTanggal"
              @change="filterStok"
              class="form-control"
            />
          </div>

          <!-- Form Tambah/Edit Data -->
          <div class="mt-4">
            <h4>{{ isEditing ? "Edit" : "Tambah" }} Stok Barang</h4>
            <form @submit.prevent="isEditing ? updateStok() : addStok()">
              <div class="mb-3">
                <label for="tanggal" class="form-label">Tanggal:</label>
                <input
                  type="date"
                  id="tanggal"
                  v-model="formData.tanggal"
                  class="form-control"
                  required
                />
              </div>
              <div class="mb-3">
                <label for="stokAwal" class="form-label">Stok Awal:</label>
                <input
                  type="number"
                  id="stokAwal"
                  v-model.number="formData.stokAwal"
                  @input="calculateStokAkhir"
                  class="form-control"
                  min="0"
                  required
                />
              </div>
              <div class="mb-3">
                <label for="jumlahMasuk" class="form-label"
                  >Jumlah Masuk:</label
                >
                <input
                  type="number"
                  id="jumlahMasuk"
                  v-model.number="formData.jumlahMasuk"
                  @input="calculateStokAkhir"
                  class="form-control"
                  min="0"
                  required
                />
              </div>
              <div class="mb-3">
                <label for="barangKeluar" class="form-label"
                  >Barang Keluar:</label
                >
                <input
                  type="number"
                  id="barangKeluar"
                  v-model.number="formData.barangKeluar"
                  @input="calculateStokAkhir"
                  class="form-control"
                  min="0"
                  required
                />
              </div>
              <div class="mb-3">
                <label for="stokAkhir" class="form-label">Stok Akhir:</label>
                <input
                  type="number"
                  id="stokAkhir"
                  v-model="formData.stokAkhir"
                  class="form-control"
                  readonly
                />
              </div>
              <div class="form-actions">
                <button type="submit" class="btn btn-primary">
                  {{ isEditing ? "Update" : "Tambah" }}
                </button>

                <button
                  type="button"
                  v-if="isEditing"
                  class="btn btn-outline-secondary"
                  @click="cancelEdit"
                >
                  Batal
                </button>
              </div>
            </form>
          </div>

          <!-- Tabel stok barang -->
          <div class="table-responsive mt-3">
            <table class="table">
              <thead>
                <tr>
                  <th>#</th>
                  <th>Tanggal</th>
                  <th>Stok Awal</th>
                  <th>Jumlah Masuk</th>
                  <th>Barang Keluar</th>
                  <th>Stok Akhir</th>
                  <th>Aksi</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, index) in stokFiltered" :key="item.id">
                  <td>{{ index + 1 }}</td>
                  <td>{{ item.tanggal }}</td>
                  <td>{{ item.stokAwal }}</td>
                  <td>{{ item.jumlahMasuk }}</td>
                  <td>{{ item.barangKeluar }}</td>
                  <td>{{ item.stokAkhir }}</td>
                  <td class="aksi">
                    <button
                      class="btn btn-outline-primary btn-sm btn-icon"
                      @click="editStok(item)"
                      title="Edit"
                    >
                      <b-icon-pencil />
                    </button>

                    <button
                      class="btn btn-outline-danger btn-sm btn-icon"
                      @click="deleteStok(item.id)"
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
import axios from "axios";

export default {
  name: "StokBarang",
  components: {
    Navbar,
  },
  data() {
    return {
      stokData: [], // Data stok barang
      filterTanggal: "", // Filter berdasarkan tanggal
      stokFiltered: [], // Data yang difilter
      formData: {
        id: null,
        tanggal: "",
        stokAwal: 0,
        jumlahMasuk: 0,
        barangKeluar: 0,
      }, // Data untuk form
      isEditing: false, // Status edit
    };
  },
  methods: {
    async loadStokBarang() {
      try {
        const response = await axios.get("http://localhost:3000/stokBarang");
        this.stokData = response.data;
        this.stokFiltered = this.stokData;
      } catch (error) {
        console.error("Error fetching stok barang:", error);
      }
    },
    filterStok() {
      this.stokFiltered = this.filterTanggal
        ? this.stokData.filter((item) => item.tanggal === this.filterTanggal)
        : this.stokData;
    },
    calculateStokAkhir() {
      this.formData.stokAkhir =
        this.formData.stokAwal +
        this.formData.jumlahMasuk -
        this.formData.barangKeluar;
    },
    async addStok() {
      try {
        const response = await axios.post(
          "http://localhost:3000/stokBarang",
          this.formData
        );
        this.stokData.push(response.data);
        this.filterStok();
        this.resetForm();
      } catch (error) {
        console.error("Error adding stok:", error);
      }
    },
    async deleteStok(id) {
      try {
        await axios.delete(`http://localhost:3000/stokBarang/${id}`);
        this.stokData = this.stokData.filter((item) => item.id !== id);
        this.filterStok();
      } catch (error) {
        console.error("Error deleting stok:", error);
      }
    },
    editStok(item) {
      this.isEditing = true;
      this.formData = { ...item };
    },
    async updateStok() {
      try {
        const response = await axios.put(
          `http://localhost:3000/stokBarang/${this.formData.id}`,
          this.formData
        );
        const index = this.stokData.findIndex(
          (item) => item.id === response.data.id
        );
        this.stokData.splice(index, 1, response.data);
        this.filterStok();
        this.resetForm();
      } catch (error) {
        console.error("Error updating stok:", error);
      }
    },
    cancelEdit() {
      this.resetForm();
    },
    resetForm() {
      this.isEditing = false;
      this.formData = {
        id: null,
        tanggal: "",
        stokAwal: 0,
        jumlahMasuk: 0,
        barangKeluar: 0,
        stokAkhir: 0,
      };
    },
  },

  mounted() {
    this.loadStokBarang();
  },
};
</script>

<style scoped>
.aksi {
  display: flex;
  gap: 8px; /* JARAK ANTAR BUTTON */
}

.btn-icon {
  padding: 6px 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.form-actions {
  display: flex;
  gap: 12px; /* JARAK ANTAR BUTTON */
  margin-top: 12px;
}
</style>
