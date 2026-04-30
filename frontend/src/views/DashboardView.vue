<template>
  <div class="dashboard">
    <header class="dashboard__header">
      <div class="header-inner">
        <div class="header-brand">
          <svg width="28" height="28" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect width="48" height="48" rx="12" fill="#6366f1"/>
            <path d="M14 24L22 32L34 16" stroke="white" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span class="header-brand__name">MyApp</span>
        </div>
        <div class="header-user">
          <span class="header-user__name">{{ authStore.user?.name }}</span>
          <button class="btn-logout" @click="handleLogout">Sign out</button>
        </div>
      </div>
    </header>

    <main class="dashboard__main">
      <div class="welcome-card">
        <h1 class="welcome-card__title">Welcome back, {{ authStore.user?.name }} 👋</h1>
        <p class="welcome-card__sub">You're successfully logged in. This is your dashboard.</p>
      </div>
    </main>
  </div>
</template>

<script setup>
import { useRouter } from 'vue-router'
import { useAuthStore } from '@/stores/auth'

const router = useRouter()
const authStore = useAuthStore()

function handleLogout() {
  authStore.logout()
  router.replace('/login')
}
</script>

<style scoped>
.dashboard {
  min-height: 100vh;
  background: #f8fafc;
}

.dashboard__header {
  background: #fff;
  border-bottom: 1px solid #e2e8f0;
  padding: 0 1.5rem;
}

.header-inner {
  max-width: 1100px;
  margin: 0 auto;
  height: 64px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.header-brand {
  display: flex;
  align-items: center;
  gap: 0.6rem;
}

.header-brand__name {
  font-size: 1.1rem;
  font-weight: 700;
  color: #0f172a;
}

.header-user {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.header-user__name {
  font-size: 0.875rem;
  color: #475569;
}

.btn-logout {
  padding: 0.45rem 1rem;
  background: none;
  border: 1.5px solid #e2e8f0;
  border-radius: 8px;
  font-size: 0.85rem;
  font-weight: 500;
  color: #374151;
  cursor: pointer;
  transition: background 0.15s, border-color 0.15s;
}

.btn-logout:hover {
  background: #f1f5f9;
  border-color: #cbd5e1;
}

.dashboard__main {
  max-width: 1100px;
  margin: 0 auto;
  padding: 2.5rem 1.5rem;
}

.welcome-card {
  background: #fff;
  border: 1px solid #e2e8f0;
  border-radius: 16px;
  padding: 2.5rem;
  box-shadow: 0 1px 3px rgba(0,0,0,0.04);
}

.welcome-card__title {
  font-size: 1.6rem;
  font-weight: 700;
  color: #0f172a;
  margin: 0 0 0.5rem;
}

.welcome-card__sub {
  color: #64748b;
  margin: 0;
}
</style>
