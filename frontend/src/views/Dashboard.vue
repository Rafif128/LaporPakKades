<script setup>
import { ref, computed, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const menuOpen = ref(false)
const currentUser = ref(null)
const filterKategori = ref('SEMUA')
const daftarAduan = ref([])

const DEFAULT_ADUAN = [
  {
    id: 1,
    nik: '3277012345670001',
    nama: 'Budi Santoso',
    alamat: 'RT 02 / RW 04 Dusun Kidul',
    kategori: 'ADUAN',
    isi: 'Jalan berlubang cukup dalam di depan Balai RW 04, membahayakan pengendara motor saat hujan.',
    status: 'Diproses',
    tanggapan: 'Tim PUPR Desa akan melakukan penambalan minggu depan.',
    tanggal: '2026-11-20'
  },
  {
    id: 2,
    nik: '3277012345670002',
    nama: 'Siti Aminah',
    alamat: 'RT 05 / RW 01 Dusun Wetan',
    kategori: 'SARAN',
    isi: 'Saran agar disediakan tempat sampah terpilah (organik dan anorganik) di area taman desa.',
    status: 'Selesai',
    tanggapan: 'Terima kasih sarannya Bu Siti, tempat sampah terpilah sudah dipasang pagi ini.',
    tanggal: '2026-11-22'
  },
  {
    id: 3,
    nik: '3277012345670003',
    nama: 'Ahmad Fauzi',
    alamat: 'RT 01 / RW 03 Dusun Kulon',
    kategori: 'ADUAN',
    isi: 'Lampu penerangan jalan umum (PJU) di gang Melati mati total sejak 3 hari lalu.',
    status: 'Menunggu',
    tanggapan: '',
    tanggal: '2026-11-25'
  }
]

onMounted(() => {
  const user = JSON.parse(localStorage.getItem('lpk_current_user'))
  if (!user) {
    router.push('/login')
    return
  }
  currentUser.value = user

  if (!localStorage.getItem('lpk_aduan')) {
    localStorage.setItem('lpk_aduan', JSON.stringify(DEFAULT_ADUAN))
  }
  daftarAduan.value = JSON.parse(localStorage.getItem('lpk_aduan')) || []
})

const filteredAduan = computed(() => {
  if (filterKategori.value === 'SEMUA') return daftarAduan.value
  return daftarAduan.value.filter(item => item.kategori === filterKategori.value)
})

const editAduan = (id) => {
  router.push({ path: '/aduan', query: { edit: id } })
}

const hapusAduan = (id) => {
  if (!confirm('Apakah Anda yakin ingin menghapus aduan ini?')) return
  daftarAduan.value = daftarAduan.value.filter(item => item.id !== id)
  localStorage.setItem('lpk_aduan', JSON.stringify(daftarAduan.value))
}

const logout = () => {
  localStorage.removeItem('lpk_current_user')
  localStorage.removeItem('lpk_token')
  router.push('/login')
}
</script>

<template>
  <div class="page-layout">
    <!-- Navbar -->
    <header class="navbar">
      <button class="menu-btn" @click="menuOpen = !menuOpen">&#9776;</button>
      <h1 class="nav-title">LAPOR PAK KADES</h1>
      <div class="user-badge">{{ currentUser?.nama || 'Warga Desa' }}</div>
    </header>

    <!-- Sidebar -->
    <nav :class="['sidebar', { open: menuOpen }]">
      <RouterLink to="/dashboard" @click="menuOpen = false">Dashboard</RouterLink>
      <RouterLink to="/aduan" @click="menuOpen = false">Buat Aduan / Saran</RouterLink>
      <RouterLink to="/about" @click="menuOpen = false">About Us</RouterLink>
      <a @click="logout" class="logout-link">Keluar (Logout)</a>
    </nav>

    <main class="container">
      <!-- Berita Desa -->
      <section style="margin-bottom: 40px;">
        <h2 class="section-heading">BERITA DESA</h2>
        <div class="news-grid">
          <div class="news-card">
            <img src="https://images.unsplash.com/photo-1542601906990-b4d3fb778b09?auto=format&fit=crop&w=500&q=80" alt="Kerja Bakti" />
            <div class="news-info">
              <h4>Kerja Bakti Minggu Bersih</h4>
              <p>Jadwal pembersihan saluran irigasi RT 01 - RT 05.</p>
            </div>
          </div>
          <div class="news-card main-news">
            <img src="https://images.unsplash.com/photo-1517048676732-d65bc937f952?auto=format&fit=crop&w=800&q=80" alt="Musyawarah Desa" />
            <div class="news-info">
              <h3>Musyawarah Perencanaan Pembangunan Desa 2026</h3>
              <p>Pemerintah desa mengundang perwakilan warga untuk membahas prioritas perbaikan jalan dan penerangan umum.</p>
            </div>
          </div>
          <div class="news-card">
            <img src="https://images.unsplash.com/photo-1581092160562-40aa08e78837?auto=format&fit=crop&w=500&q=80" alt="PJU" />
            <div class="news-info">
              <h4>Perbaikan PJU Tahap 2</h4>
              <p>Pemasangan lampu jalan tenaga surya di jalan utama desa.</p>
            </div>
          </div>
        </div>
      </section>

      <!-- Daftar Aduan Masyarakat -->
      <section style="margin-bottom: 40px;">
        <div class="section-header-flex">
          <h2 class="section-heading" style="margin-bottom: 0;">ADUAN & SARAN MASYARAKAT</h2>
          <select v-model="filterKategori" class="filter-select">
            <option value="SEMUA">Semua Laporan</option>
            <option value="ADUAN">Hanya Aduan</option>
            <option value="SARAN">Hanya Saran</option>
          </select>
        </div>

        <div class="aduan-grid">
          <div
            v-for="item in filteredAduan"
            :key="item.id"
            :class="['aduan-card', { 'saran-type': item.kategori === 'SARAN' }]"
          >
            <div>
              <div class="card-top">
                <span :class="['badge-type', { saran: item.kategori === 'SARAN' }]">{{ item.kategori }}</span>
                <span :class="['badge-status', { selesai: item.status === 'Selesai' }]">{{ item.status }}</span>
              </div>
              <div class="aduan-meta">
                <strong>{{ item.nama }}</strong> &bull; {{ item.alamat }}<br />
                <small>Tanggal: {{ item.tanggal }}</small>
              </div>
              <p class="aduan-content">"{{ item.isi }}"</p>
              <div v-if="item.tanggapan" class="tanggapan-box">
                <strong>Tanggapan Kades:</strong> {{ item.tanggapan }}
              </div>
            </div>

            <!-- Tombol Edit & Hapus khusus pemilik laporan yang berstatus Menunggu -->
            <div v-if="currentUser?.nik === item.nik && item.status === 'Menunggu'" class="card-actions">
              <button class="btn-edit" @click="editAduan(item.id)">Edit</button>
              <button class="btn-delete" @click="hapusAduan(item.id)">Hapus</button>
            </div>
          </div>
        </div>

        <div class="cta-box">
          <p><strong>Buat aduan atau saranmu, sekarang!</strong></p>
          <RouterLink to="/aduan" class="btn-primary">Buat sekarang</RouterLink>
        </div>
      </section>
    </main>

    <footer class="footer">DESA NUSAPUTRA - SISTEM PENGADUAN ONLINE &copy; 2026</footer>
  </div>
</template>

<style scoped>
.section-header-flex {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 18px;
}
.filter-select {
  padding: 8px 14px;
  border-radius: 8px;
  border: 1px solid #9ca3af;
  background: #fff;
  font-weight: 600;
}
.news-grid {
  display: grid;
  grid-template-columns: 1fr 1.8fr 1fr;
  gap: 18px;
}
.news-card {
  background: #3f3838;
  color: #fff;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}
.news-card img {
  width: 100%;
  height: 160px;
  object-fit: cover;
}
.main-news img {
  height: 200px;
}
.news-info {
  padding: 14px;
}
.news-info h3, .news-info h4 {
  margin-bottom: 6px;
  color: #bbf7d0;
}
.news-info p {
  font-size: 0.85rem;
  color: #e5e7eb;
}
.aduan-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(310px, 1fr));
  gap: 20px;
}
.aduan-card {
  background: #fff;
  border-radius: 12px;
  padding: 18px;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
  border-top: 5px solid var(--primary-green);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.aduan-card.saran-type {
  border-top-color: #0284c7;
}
.card-top {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}
.badge-type {
  font-size: 0.75rem;
  font-weight: bold;
  padding: 3px 10px;
  border-radius: 12px;
  background: #dcfce7;
  color: #166534;
}
.badge-type.saran {
  background: #e0f2fe;
  color: #075985;
}
.badge-status {
  font-size: 0.75rem;
  font-weight: 600;
  padding: 3px 10px;
  border-radius: 12px;
  background: #fef3c7;
  color: #92400e;
}
.badge-status.selesai {
  background: #d1fae5;
  color: #065f46;
}
.aduan-meta {
  font-size: 0.82rem;
  color: var(--text-muted);
  margin-bottom: 10px;
}
.aduan-content {
  font-size: 0.95rem;
  line-height: 1.5;
  margin-bottom: 14px;
}
.tanggapan-box {
  background: #f3f4f6;
  border-left: 3px solid var(--primary-green);
  padding: 8px 12px;
  font-size: 0.83rem;
  margin-bottom: 12px;
  border-radius: 4px;
}
.card-actions {
  display: flex;
  gap: 8px;
  border-top: 1px solid #e5e7eb;
  padding-top: 10px;
}
.btn-edit {
  padding: 5px 12px;
  border: none;
  border-radius: 6px;
  font-size: 0.8rem;
  font-weight: 600;
  cursor: pointer;
  background: #fef08a;
  color: #854d0e;
}
.btn-delete {
  padding: 5px 12px;
  border: none;
  border-radius: 6px;
  font-size: 0.8rem;
  font-weight: 600;
  cursor: pointer;
  background: #fee2e2;
  color: #991b1b;
}
.cta-box {
  text-align: center;
  margin-top: 32px;
}
.cta-box p {
  margin-bottom: 12px;
  font-size: 1.05rem;
}
@media (max-width: 768px) {
  .news-grid {
    grid-template-columns: 1fr;
  }
}
</style>