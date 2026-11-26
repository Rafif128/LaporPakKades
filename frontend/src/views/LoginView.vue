<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const nik = ref('')
const password = ref('')

const slides = [
  {
    img: 'https://images.unsplash.com/photo-1596422846543-75c6fc197f07?auto=format&fit=crop&w=800&q=80',
    caption: 'Sampaikan aspirasi demi kemajuan desa kita bersama.'
  },
  {
    img: 'https://images.unsplash.com/photo-1542601906990-b4d3fb778b09?auto=format&fit=crop&w=800&q=80',
    caption: 'Laporkan kerusakan fasilitas umum dengan cepat dan transparan.'
  },
  {
    img: 'https://images.unsplash.com/photo-1517048676732-d65bc937f952?auto=format&fit=crop&w=800&q=80',
    caption: 'Komunikasi langsung antara warga dan perangkat desa.'
  }
]

const currentSlide = ref(0)
let slideTimer = null

onMounted(() => {
  // Inisialisasi akun default jika belum ada
  if (!localStorage.getItem('lpk_users')) {
    localStorage.setItem('lpk_users', JSON.stringify([
      {
        nama: 'Warga Contoh',
        tgl_lahir: '1998-05-12',
        nik: '1234567890123456',
        jk: 'Laki-laki',
        alamat: 'RT 03 / RW 02 Desa Nusaputra',
        email: 'warga@desa.id',
        telepon: '081234567890',
        password: '123'
      }
    ]))
  }

  slideTimer = setInterval(() => {
    currentSlide.value = (currentSlide.value + 1) % slides.length
  }, 4000)
})

onUnmounted(() => {
  clearInterval(slideTimer)
})

const handleLogin = async () => {
  // Simulasi / Integrasi POST /api/auth/login
  const users = JSON.parse(localStorage.getItem('lpk_users')) || []
  const foundUser = users.find(u => u.nik === nik.value && u.password === password.value)

  if (foundUser) {
    localStorage.setItem('lpk_current_user', JSON.stringify(foundUser))
    localStorage.setItem('lpk_token', 'mock-jwt-token-2026')
    router.push('/dashboard')
  } else {
    alert('NIK atau Password tidak sesuai! (Coba NIK: 1234567890123456, Pass: 123)')
  }
}
</script>

<template>
  <div class="auth-wrapper">
    <div class="auth-container">
      <!-- Kiri: Judul & Slider -->
      <div class="auth-left">
        <h1 class="brand-title">LAPOR PAK KADES!</h1>
        <p class="brand-subtitle">LAYANAN ADUAN & SARAN MASYARAKAT ONLINE</p>

        <div class="slider-box">
          <img :src="slides[currentSlide].img" alt="Kegiatan Desa" />
          <div class="slider-caption">{{ slides[currentSlide].caption }}</div>
        </div>
      </div>

      <!-- Kanan: Form Login -->
      <div class="auth-right">
        <div class="auth-card">
          <h2>LOGIN</h2>
          <form @submit.prevent="handleLogin">
            <div class="form-group">
              <input v-model="nik" type="text" placeholder="NIK (16 Digit)" maxlength="16" required />
            </div>
            <div class="form-group">
              <input v-model="password" type="password" placeholder="Password" required />
            </div>
            <p class="switch-text">
              Belum punya akun?
              <RouterLink to="/register" class="switch-link">REGISTER</RouterLink>
            </p>
            <button type="submit" class="btn-submit">SUBMIT</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>