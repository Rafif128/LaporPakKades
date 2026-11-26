<script setup>
import { reactive } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const form = reactive({
  nama: '',
  tgl_lahir: '',
  nik: '',
  jk: '',
  alamat: '',
  email: '',
  telepon: '',
  password: ''
})

const handleRegister = async () => {
  // Simulasi / Integrasi POST /api/auth/register
  const users = JSON.parse(localStorage.getItem('lpk_users')) || []

  if (users.some(u => u.nik === form.nik)) {
    alert('NIK sudah terdaftar! Silakan langsung login.')
    return
  }

  users.push({ ...form })
  localStorage.setItem('lpk_users', JSON.stringify(users))

  alert('Registrasi berhasil! Silakan login menggunakan NIK dan Password Anda.')
  router.push('/login')
}
</script>

<template>
  <div class="auth-wrapper">
    <div class="auth-container">
      <!-- Kiri: Banner -->
      <div class="auth-left">
        <h1 class="brand-title">LAPOR PAK KADES!</h1>
        <p class="brand-subtitle">LAYANAN ADUAN & SARAN MASYARAKAT ONLINE</p>

        <div class="slider-box">
          <img
            src="https://images.unsplash.com/photo-1596422846543-75c6fc197f07?auto=format&fit=crop&w=800&q=80"
            alt="Desa"
          />
          <div class="slider-caption">Daftarkan identitas kependudukan Anda untuk mulai melapor.</div>
        </div>
      </div>

      <!-- Kanan: Form Register 8 Kolom -->
      <div class="auth-right">
        <div class="auth-card">
          <h2>REGISTER</h2>
          <form @submit.prevent="handleRegister">
            <div class="form-group">
              <input v-model="form.nama" type="text" placeholder="Nama Lengkap" required />
            </div>
            <div class="form-group">
              <input v-model="form.tgl_lahir" type="date" title="Tanggal Lahir" required />
            </div>
            <div class="form-group">
              <input v-model="form.nik" type="text" placeholder="Nomor NIK (16 Digit)" maxlength="16" required />
            </div>
            <div class="form-group">
              <select v-model="form.jk" required>
                <option value="" disabled>Pilih Jenis Kelamin</option>
                <option value="Laki-laki">Laki-laki</option>
                <option value="Perempuan">Perempuan</option>
              </select>
            </div>
            <div class="form-group">
              <input v-model="form.alamat" type="text" placeholder="Alamat Tempat Tinggal (RT/RW/Dusun)" required />
            </div>
            <div class="form-group">
              <input v-model="form.email" type="email" placeholder="Email (Opsional)" />
            </div>
            <div class="form-group">
              <input v-model="form.telepon" type="tel" placeholder="Nomor Telepon / WhatsApp" required />
            </div>
            <div class="form-group">
              <input v-model="form.password" type="password" placeholder="Password Akun" required />
            </div>

            <p class="switch-text">
              Sudah punya akun?
              <RouterLink to="/login" class="switch-link">LOGIN</RouterLink>
            </p>
            <button type="submit" class="btn-submit">SUBMIT</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>