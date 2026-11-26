<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const menuOpen = ref(false)
const currentUser = ref(null)

const teamMembers = [
  {
    nama: 'Haura Sakho Alifia Budiyana',
    nim: '2550081093',
    role: 'FrontEnd Developer',
    type: 'frontend',
    avatar: 'https://ui-avatars.com/api/?name=Haura+Sakho&background=2e7d32&color=fff&size=200',
    jobdesc: 'Bertanggung jawab pada perancangan antarmuka pengguna (UI/UX), responsivitas halaman, serta implementasi Vue & CSS.'
  },
  {
    nama: 'Fatih Rafif Anugrah',
    nim: '2550081099',
    role: 'BackEnd Developer',
    type: 'backend',
    avatar: 'https://ui-avatars.com/api/?name=Fatih+Rafif&background=1b5e20&color=fff&size=200',
    jobdesc: 'Bertanggung jawab pada arsitektur server, perancangan basis data, dan pengembangan REST API autentikasi.'
  },
  {
    nama: "Shalwa Nur'aini Fadillah",
    nim: '2550081100',
    role: 'FrontEnd Developer',
    type: 'frontend',
    avatar: 'https://ui-avatars.com/api/?name=Shalwa+Nuraini&background=2e7d32&color=fff&size=200',
    jobdesc: 'Bertanggung jawab pada integrasi REST API ke sisi klien, manajemen state interaktif, dan pengelolaan form aduan.'
  },
  {
    nama: 'Rhadian Arya Dwi Putera Senjaya',
    nim: '2550081112',
    role: 'BackEnd Developer',
    type: 'backend',
    avatar: 'https://ui-avatars.com/api/?name=Rhadian+Arya&background=1b5e20&color=fff&size=200',
    jobdesc: 'Bertanggung jawab pada endpoint pengelolaan data aduan (CRUD), manajemen hak akses user, dan keamanan sistem.'
  }
]

onMounted(() => {
  currentUser.value = JSON.parse(localStorage.getItem('lpk_current_user'))
})

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
      <h2 class="section-heading">ABOUT US</h2>
      <p class="about-intro">
        Aplikasi <strong>"Lapor Pak Kades!"</strong> dikembangkan oleh Kelompok 2 Program Studi Informatika, Fakultas Sains dan Informatika, Universitas Jenderal Achmad Yani (2026).
      </p>

      <div class="team-list">
        <div
          v-for="(member, index) in teamMembers"
          :key="member.nim"
          :class="['team-item', { 'right-align': index % 2 === 1 }]"
        >
          <img :src="member.avatar" :alt="member.nama" class="team-photo" />
          <div class="team-details">
            <h3>{{ member.nama }}</h3>
            <p class="nim">NIM: {{ member.nim }}</p>
            <span :class="['role-badge', member.type]">{{ member.role }}</span>
            <p class="jobdesc">{{ member.jobdesc }}</p>
          </div>
        </div>
      </div>
    </main>

    <footer class="footer">DESA NUSAPUTRA - SISTEM PENGADUAN ONLINE &copy; 2026</footer>
  </div>
</template>

<style scoped>
.about-intro {
  text-align: center;
  max-width: 720px;
  margin: 0 auto 35px auto;
  color: #374151;
  line-height: 1.6;
}
.team-list {
  display: flex;
  flex-direction: column;
  gap: 24px;
  max-width: 850px;
  margin: 0 auto;
}
.team-item {
  display: flex;
  align-items: center;
  gap: 24px;
  background: #fff;
  padding: 20px;
  border-radius: 14px;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.07);
}
.team-item.right-align {
  flex-direction: row-reverse;
  text-align: right;
}
.team-photo {
  width: 120px;
  height: 120px;
  border-radius: 12px;
  object-fit: cover;
  flex-shrink: 0;
}
.team-details h3 {
  color: var(--dark-green);
  font-size: 1.2rem;
}
.nim {
  font-weight: 700;
  color: #4b5563;
  margin: 4px 0 8px 0;
}
.role-badge {
  display: inline-block;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: bold;
  color: #fff;
  margin-bottom: 8px;
}
.role-badge.frontend {
  background: #2563eb;
}
.role-badge.backend {
  background: #059669;
}
.jobdesc {
  font-size: 0.9rem;
  color: #4b5563;
  line-height: 1.5;
}
@media (max-width: 768px) {
  .team-item,
  .team-item.right-align {
    flex-direction: column;
    text-align: center;
  }
}
</style>