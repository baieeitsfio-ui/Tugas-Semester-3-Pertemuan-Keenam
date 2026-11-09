<script setup>
import { ref, computed } from 'vue'

// Variabel untuk input buku
const judul = ref('')
const kategori = ref('')
const statusBaca = ref(false)

// Menyimpan daftar buku
const daftarBuku = ref([])

// Fungsi menambahkan buku
function tambahBuku() {
  if (judul.value.trim() === '') {
    alert('Judul buku harus diisi!')
    return
  }

  if (kategori.value === '') {
    alert('Silakan pilih kategori buku!')
    return
  }

  daftarBuku.value.push({
    id: Date.now(),
    judul: judul.value,
    kategori: kategori.value,
    selesai: statusBaca.value
  })

  judul.value = ''
  kategori.value = ''
  statusBaca.value = false
}

// Menghitung buku yang selesai dibaca
const jumlahSelesai = computed(() => {
  return daftarBuku.value.filter(
    buku => buku.selesai
  ).length
})
</script>

<template>
  <div class="container">
    <h1>📚 Buku Catatan</h1>
    <p>Aplikasi pencatatan daftar buku bacaan</p>

    <div class="form-buku">
      <h2>Tambah Buku</h2>

      <form @submit.prevent="tambahBuku">
        <label>Judul Buku</label>
        <input
          type="text"
          v-model="judul"
          placeholder="Masukkan judul buku"
        />

        <label>Kategori Buku</label>
        <select v-model="kategori">
          <option value="">Pilih kategori</option>
          <option value="Novel">Novel</option>
          <option value="Pendidikan">Pendidikan</option>
          <option value="Teknologi">Teknologi</option>
          <option value="Sejarah">Sejarah</option>
          <option value="Lainnya">Lainnya</option>
        </select>

        <label>
          <input type="checkbox" v-model="statusBaca" />
          Sudah selesai dibaca
        </label>

        <button type="submit">+ Tambahkan Buku</button>
      </form>
    </div>

    <div class="hasil">
      <h2>Daftar Buku</h2>

      <p>
        Buku selesai dibaca:
        <strong>{{ jumlahSelesai }}</strong>
      </p>

      <p v-if="daftarBuku.length === 0">
        Belum ada buku yang ditambahkan.
      </p>

      <div
        v-for="(buku, index) in daftarBuku"
        :key="buku.id"
        class="item-buku"
      >
        <h3>{{ index + 1 }}. {{ buku.judul }}</h3>
        <p>Kategori: {{ buku.kategori }}</p>

        <label>
          <input type="checkbox" v-model="buku.selesai" />
          {{ buku.selesai ? 'Sudah selesai dibaca' : 'Belum selesai dibaca' }}
        </label>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 700px;
  margin: 30px auto;
  padding: 25px;
  font-family: Arial, sans-serif;
  color: #263238;
}

h1 {
  text-align: center;
  color: #287b58;
}

.form-buku, .hasil {
  background: #f5faf7;
  padding: 20px;
  border-radius: 12px;
  margin-top: 20px;
}

input[type="text"], select {
  width: 100%;
  padding: 11px;
  margin: 8px 0;
  border: 1px solid #ccc;
  border-radius: 7px;
  box-sizing: border-box;
}

button {
  width: 100%;
  padding: 12px;
  margin-top: 15px;
  background: #287b58;
  color: white;
  border: none;
  border-radius: 7px;
  cursor: pointer;
}

.item-buku {
  background: white;
  padding: 12px;
  border: 1px solid #dce8df;
  border-radius: 9px;
  margin-top: 12px;
}
</style>