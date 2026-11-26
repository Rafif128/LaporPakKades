
<script setup>
import { ref } from 'vue'

const nama = ref('')
const kategori = ref('')
const lokasi = ref('')
const isiLaporan = ref('')

const laporan = ref([])

function kirimLaporan() {
  if (!nama.value || !kategori.value || !lokasi.value || !isiLaporan.value) {
    alert('Mohon lengkapi semua data laporan!')
    return
  }

  laporan.value.unshift({
    id: Date.now(),
    nama: nama.value,
    kategori: kategori.value,
    lokasi: lokasi.value,
    isi: isiLaporan.value,
    tanggal: new Date().toLocaleDateString('id-ID'),
    status: 'Menunggu'
  })

  nama.value = ''
  kategori.value = ''
  lokasi.value = ''
  isiLaporan.value = ''

  alert('Laporan berhasil dibuat!')
}
</script>

<template>
  <div class="app">
    <!-- NAVBAR -->
    <header class="navbar">
      <div class="brand">
        <div class="brand-icon">LP</div>
        <span>Lapor<span class="green">PakKades</span></span>
      </div>

      <nav>
        <a href="#beranda">Beranda</a>
        <a href="#laporan">Buat Laporan</a>
        <a href="#daftar">Daftar Laporan</a>
      </nav>

      <a href="#laporan" class="nav-button">Laporkan</a>
    </header>

    <!-- HERO -->
    <section id="beranda" class="hero">
      <div class="hero-content">
        <span class="badge">Pelayanan Masyarakat Digital</span>

        <h1>
          Sampaikan Aspirasi,<br />
          <span>Bangun Desa Bersama.</span>
        </h1>

        <p>
          LaporPakKades adalah platform pengaduan masyarakat
          untuk menyampaikan keluhan, aspirasi, dan permasalahan
          di lingkungan desa secara mudah.
        </p>

        <a href="#laporan" class="hero-button">
          Buat Laporan Sekarang →
        </a>

        <div class="hero-info">
          <div>
            <strong>01</strong>
            <span>Mudah digunakan</span>
          </div>
          <div>
            <strong>02</strong>
            <span>Pelaporan digital</span>
          </div>
          <div>
            <strong>03</strong>
            <span>Untuk warga desa</span>
          </div>
        </div>
      </div>

      <div class="hero-illustration">
        <div class="illustration-card">
          <div class="illustration-icon">🏡</div>
          <h3>Desa Lebih Baik</h3>
          <p>Dimulai dari suara masyarakat</p>

          <div class="mini-report">
            <span class="mini-dot"></span>
            Laporan warga
            <span class="mini-status">Terkirim</span>
          </div>

          <div class="mini-report">
            <span class="mini-dot orange"></span>
            Aspirasi desa
            <span class="mini-status">Diproses</span>
          </div>
        </div>
      </div>
    </section>

    <!-- FORM LAPORAN -->
    <section id="laporan" class="section">
      <div class="section-heading">
        <span class="badge">Form Pengaduan</span>
        <h2>Sampaikan Laporan Anda</h2>
        <p>Isi informasi berikut dengan jelas agar laporan mudah dipahami.</p>
      </div>

      <div class="form-card">
        <form @submit.prevent="kirimLaporan">
          <div class="form-grid">
            <div class="form-group">
              <label>Nama Pelapor</label>
              <input
                v-model="nama"
                type="text"
                placeholder="Masukkan nama Anda"
              />
            </div>

            <div class="form-group">
              <label>Kategori Laporan</label>
              <select v-model="kategori">
                <option value="">Pilih kategori</option>
                <option>Infrastruktur</option>
                <option>Kebersihan</option>
                <option>Pelayanan Publik</option>
                <option>Keamanan</option>
                <option>Sosial</option>
                <option>Lainnya</option>
              </select>
            </div>

            <div class="form-group full">
              <label>Lokasi Kejadian</label>
              <input
                v-model="lokasi"
                type="text"
                placeholder="Contoh: Jalan Desa, RT 02 RW 04"
              />
            </div>

            <div class="form-group full">
              <label>Isi Laporan</label>
              <textarea
                v-model="isiLaporan"
                rows="5"
                placeholder="Jelaskan permasalahan yang ingin dilaporkan..."
              ></textarea>
            </div>
          </div>

          <button type="submit" class="submit-button">
            Kirim Laporan →
          </button>
        </form>
      </div>
    </section>

    <!-- DAFTAR LAPORAN -->
    <section id="daftar" class="section report-section">
      <div class="section-heading">
        <span class="badge">Pengaduan</span>
        <h2>Daftar Laporan</h2>
        <p>Laporan yang dibuat melalui form akan muncul di bawah ini.</p>
      </div>

      <div v-if="laporan.length === 0" class="empty-state">
        <div class="empty-icon">📋</div>
        <h3>Belum ada laporan</h3>
        <p>Laporan yang kamu kirim akan ditampilkan di sini.</p>
      </div>

      <div v-else class="report-list">
        <div
          v-for="item in laporan"
          :key="item.id"
          class="report-card"
        >
          <div class="report-top">
            <span class="report-category">{{ item.kategori }}</span>
            <span class="report-status">{{ item.status }}</span>
          </div>

          <h3>{{ item.lokasi }}</h3>
          <p>{{ item.isi }}</p>

          <div class="report-footer">
            <span>Pelapor: {{ item.nama }}</span>
            <span>{{ item.tanggal }}</span>
          </div>
        </div>
      </div>
    </section>

    <!-- FOOTER -->
    <footer class="footer">
      <div class="brand">
        <div class="brand-icon">LP</div>
        <span>Lapor<span class="green">PakKades</span></span>
      </div>

      <p>Suara warga, perubahan desa.</p>
      <small>© 2026 LaporPakKades</small>
    </footer>
  </div>
</template>

<style>
* {
  box-sizing: border-box;
  scroll-behavior: smooth;
}

body {
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
  color: #1f2937;
  background: #f8fafc;
}

.app {
  min-height: 100vh;
}

.navbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 8%;
  background: white;
  border-bottom: 1px solid #e5e7eb;
}

.brand {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 21px;
  font-weight: 700;
}

.brand-icon {
  display: grid;
  place-items: center;
  width: 38px;
  height: 38px;
  color: white;
  background: #16834a;
  border-radius: 10px;
  font-size: 14px;
}

.green {
  color: #16834a;
}

.navbar nav {
  display: flex;
  gap: 30px;
}

.navbar nav a {
  color: #475569;
  text-decoration: none;
  font-size: 14px;
}

.navbar nav a:hover {
  color: #16834a;
}

.nav-button,
.hero-button {
  display: inline-block;
  padding: 12px 20px;
  color: white;
  background: #16834a;
  border-radius: 8px;
  text-decoration: none;
  font-weight: 600;
}

.hero {
  display: grid;
  grid-template-columns: 1.2fr 0.8fr;
  align-items: center;
  gap: 50px;
  padding: 80px 8%;
  background: #eef8f1;
}

.hero-content {
  max-width: 650px;
}

.badge {
  display: inline-block;
  padding: 8px 13px;
  color: #16834a;
  background: #d9f2e2;
  border-radius: 30px;
  font-size: 12px;
  font-weight: 700;
}

.hero h1 {
  margin: 22px 0;
  font-size: clamp(35px, 4vw, 55px);
  line-height: 1.2;
  color: #173b2a;
}

.hero h1 span {
  color: #16834a;
}

.hero p {
  max-width: 550px;
  color: #64748b;
  line-height: 1.8;
}

.hero-button {
  margin-top: 20px;
}

.hero-info {
  display: flex;
  gap: 35px;
  margin-top: 45px;
}

.hero-info div {
  display: flex;
  flex-direction: column;
  gap: 7px;
}

.hero-info strong {
  color: #16834a;
  font-size: 22px;
}

.hero-info span {
  color: #64748b;
  font-size: 12px;
}

.hero-illustration {
  display: flex;
  justify-content: center;
}

.illustration-card {
  width: 100%;
  max-width: 350px;
  padding: 35px;
  background: white;
  border-radius: 20px;
  box-shadow: 0 15px 45px #174d2b12;
  text-align: center;
}

.illustration-icon {
  display: grid;
  place-items: center;
  width: 100px;
  height: 100px;
  margin: auto;
  background: #e4f5e9;
  border-radius: 50%;
  font-size: 50px;
}

.illustration-card h3 {
  margin-bottom: 5px;
}

.illustration-card p {
  margin: 0 0 25px;
  font-size: 13px;
}

.mini-report {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-top: 12px;
  padding: 12px;
  background: #f8fafc;
  border-radius: 8px;
  font-size: 12px;
}

.mini-dot {
  width: 8px;
  height: 8px;
  background: #16834a;
  border-radius: 50%;
}

.orange {
  background: #f59e0b;
}

.mini-status {
  margin-left: auto;
  color: #16834a;
}

.section {
  padding: 75px 8%;
}

.section-heading {
  margin-bottom: 35px;
  text-align: center;
}

.section-heading h2 {
  margin: 15px 0 10px;
  font-size: 32px;
  color: #173b2a;
}

.section-heading p {
  color: #64748b;
  font-size: 14px;
}

.form-card {
  max-width: 800px;
  margin: auto;
  padding: 35px;
  background: white;
  border: 1px solid #e5e7eb;
  border-radius: 15px;
  box-shadow: 0 10px 30px #00000005;
}

.form-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 22px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 9px;
}

.form-group.full {
  grid-column: 1 / -1;
}

.form-group label {
  font-size: 14px;
  font-weight: 600;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 13px;
  border: 1px solid #dbe2e8;
  border-radius: 8px;
  font: inherit;
  font-size: 14px;
  background: white;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: 2px solid #b8e4c7;
  border-color: #16834a;
}

.form-group textarea {
  resize: vertical;
}

.submit-button {
  margin-top: 25px;
  padding: 14px 24px;
  color: white;
  background: #16834a;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
}

.submit-button:hover,
.nav-button:hover,
.hero-button:hover {
  background: #116b3b;
}

.report-section {
  background: #eef8f1;
}

.empty-state {
  max-width: 600px;
  margin: auto;
  padding: 45px;
  background: white;
  border-radius: 15px;
  text-align: center;
}

.empty-icon {
  font-size: 40px;
}

.empty-state p {
  color: #64748b;
  font-size: 14px;
}

.report-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 20px;
}

.report-card {
  padding: 25px;
  background: white;
  border: 1px solid #e5e7eb;
  border-radius: 12px;
}

.report-top {
  display: flex;
  justify-content: space-between;
  gap: 10px;
}

.report-category {
  color: #16834a;
  font-size: 12px;
  font-weight: 700;
}

.report-status {
  padding: 5px 9px;
  color: #92400e;
  background: #fef3c7;
  border-radius: 20px;
  font-size: 11px;
}

.report-card h3 {
  margin: 18px 0 10px;
}

.report-card p {
  color: #64748b;
  font-size: 14px;
  line-height: 1.6;
}

.report-footer {
  display: flex;
  flex-direction: column;
  gap: 7px;
  margin-top: 20px;
  padding-top: 15px;
  border-top: 1px solid #e5e7eb;
  color: #64748b;
  font-size: 12px;
}

.footer {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
  padding: 35px;
  background: white;
  text-align: center;
}

.footer p {
  margin: 0;
  color: #64748b;
  font-size: 14px;
}

.footer small {
  color: #94a3b8;
}

@media (max-width: 750px) {
  .navbar {
    flex-wrap: wrap;
    gap: 18px;
    padding: 18px 5%;
  }

  .navbar nav {
    order: 3;
    width: 100%;
    justify-content: center;
    gap: 18px;
  }

  .navbar nav a {
    font-size: 12px;
  }

  .nav-button {
    padding: 10px 14px;
    font-size: 13px;
  }

  .hero {
    grid-template-columns: 1fr;
    padding: 55px 6%;
  }

  .hero-info {
    flex-wrap: wrap;
    gap: 20px;
  }

  .section {
    padding: 55px 6%;
  }

  .form-card {
    padding: 22px;
  }

  .form-grid {
    grid-template-columns: 1fr;
  }

  .form-group.full {
    grid-column: auto;
  }
}
</style>
