<script setup>
import { ref, onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()

const menuOpen = ref(false)
const currentUser = ref(null)
const editId = ref(null)

const kategori = ref('ADUAN')
const isiLaporan = ref('')

onMounted(() => {
  const user = JSON.parse(localStorage.getItem('lpk_current_user'))
  if (!user) {
    router.push('/login')
    return
  }
  currentUser.value = user

  // Cek mode Edit
  if (route.query.edit) {
    editId.value = route.query.edit
    const daftarAduan = JSON.parse(localStorage.getItem('lpk_aduan')) || []
    const target = daftarAduan.find(item => item.id == editId.value)
    if (target) {
      kategori.value = target.kategori
      isiLaporan.value = target.isi
    }
  }
})

const submitAduan = () => {
  let daftarAduan = JSON.parse(localStorage.getItem('lpk_aduan')) || []

  if (editId.value) {
    // Update (PUT /api/aduan/{id})
    daftarAduan = daftarAduan.map(item =>
      item.id == editId.value ? { ...item, kategori: kategori.value, isi: isiLaporan.value } : item
    )
    alert('Aduan berhasil diperbarui!')
  } else {
    // Create (POST /api/aduan)
    daftarAduan.unshift({
      id: Date.now(),
      nik: currentUser.value.nik,
      nama: currentUser.value.nama,
      alamat: currentUser.value.alamat,
      kategori: kategori.value,
      isi: isiLaporan.value,
      status: 'Menunggu',
      tanggapan: '',
      tanggal: new Date().toISOString().split('T')[0]
    })
    alert('Laporan berhasil dikirim!')
  }

  localStorage.setItem('lpk_aduan', JSON.stringify(daftarAduan))
  router.push('/dashboard')
}

const logout = () => {
  localStorage.removeItem('lpk_current_user')
  localStorage.removeItem('lpk_token')
  router.push('/login')
}
</script>

<template>
  <div class="page-layout">
    <header class="navbar">
      <button class="menu-btn" @click="menuOpen = !menuOpen">&#9776;</button>
      <h1 class="nav-title">LAPOR PAK KADES</h1>
      <div class="user-badge">{{ currentUser?.nama || 'Warga Desa' }}</div>
    </header>

    <nav :class="['sidebar', { open: menuOpen }]">
      <RouterLink to="/dashboard" @click="menuOpen = false">Dashboard</RouterLink>
      <RouterLink to="/aduan" @click="menuOpen = false">Buat Aduan / Saran</RouterLink>
      <RouterLink to="/about" @click="menuOpen = false">About Us</RouterLink>
      <a @click="logout" class="logout-link">Keluar (Logout)</a>
    </nav>

    <main class="container">
      <div class="form-container-card">
        <h2 class="section-heading">{{ editId ? 'EDIT ADUAN / SARAN' : 'BUAT ADUAN / SARAN' }}</h2>

        <form @submit.prevent="submitAduan">
          <div class="form-group">
            <label>NIK Pelapor</label>
            <input :value="currentUser?.nik" type="text" readonly class="readonly-input" />
          </div>

          <div class="form-group">
            <label>Nama Lengkap</label>
            <input :value="currentUser?.nama" type="text" readonly class="readonly-input" />
          </div>

          <div class="form-group">
            <label>Alamat Tempat Tinggal</label>
            <input :value="currentUser?.alamat" type="text" readonly class="readonly-input" />
          </div>

          <div class="radio-group-container">
            <label>Opsi laporan :</label>
            <div class="radio-options">
              <label class="radio-label">
                <input v-model="kategori" type="radio" value="ADUAN" /> ADUAN
              </label>
              <label class="radio-label">
                <input v-model="kategori" type="radio" value="SARAN" /> SARAN
              </label>
            </div>
          </div>

          <div class="form-group">
            <label>Isi laporan anda</label>
            <textarea
              v-model="isiLaporan"
              rows="7"
              placeholder="Tuliskan keluhan fasilitas umum atau saran pembangunan desa..."
              required
            ></textarea>
          </div>

          <div class="form-actions">
            <RouterLink to="/dashboard" class="btn-secondary">Batal</RouterLink>
            <button type="submit" class="btn-primary">KIRIM</button>
          </div>
        </form>
      </div>
    </main>

    <footer class="footer">DESA NUSAPUTRA - SISTEM PENGADUAN ONLINE &copy; 2026</footer>
  </div>
</template>

<style scoped>
.form-container-card {
  max-width: 650px;
  margin: 0 auto;
  background: #fff;
  padding: 28px;
  border-radius: 14px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
}
.form-group {
  margin-bottom: 16px;
}
.form-group label {
  display: block;
  font-weight: 600;
  margin-bottom: 6px;
  font-size: 0.9rem;
}
.readonly-input {
  width: 100%;
  padding: 11px 14px;
  background: #b5adad;
  color: #1f2937;
  font-weight: 600;
  border: none;
  border-radius: 8px;
}
.radio-group-container {
  text-align: center;
  margin: 20px 0;
}
.radio-options {
  display: flex;
  justify-content: center;
  gap: 30px;
  margin-top: 8px;
}
.radio-label {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  font-weight: 600;
}
.radio-label input {
  accent-color: var(--primary-green);
  width: 18px;
  height: 18px;
}
textarea {
  width: 100%;
  padding: 14px;
  background: #e5e7eb;
  border: 1px solid #9ca3af;
  border-radius: 10px;
  font-size: 0.95rem;
  resize: vertical;
}
.form-actions {
  display: flex;
  justify-content: center;
  gap: 14px;
  margin-top: 20px;
}
</style>