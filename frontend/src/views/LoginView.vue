<template>
  <div class="login-page">
    <!-- Left panel – branding -->
    <div class="login-page__brand" aria-hidden="true">
      <div class="brand-content">
        <div class="brand-logo">
          <svg width="48" height="48" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect width="48" height="48" rx="12" fill="white" fill-opacity="0.15"/>
            <path d="M14 24L22 32L34 16" stroke="white" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </div>
        <h1 class="brand-title">Welcome back</h1>
        <p class="brand-subtitle">Sign in to continue to your dashboard and manage everything in one place.</p>
        <div class="brand-features">
          <div class="feature-item" v-for="feature in features" :key="feature.text">
            <span class="feature-icon">{{ feature.icon }}</span>
            <span>{{ feature.text }}</span>
          </div>
        </div>
      </div>
      <div class="brand-circles" aria-hidden="true">
        <div class="circle circle--1"></div>
        <div class="circle circle--2"></div>
        <div class="circle circle--3"></div>
      </div>
    </div>

    <!-- Right panel – form -->
    <div class="login-page__form-panel">
      <div class="form-wrapper">
        <!-- Header -->
        <div class="form-header">
          <div class="form-logo" aria-label="App logo">
            <svg width="32" height="32" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
              <rect width="48" height="48" rx="12" fill="#6366f1"/>
              <path d="M14 24L22 32L34 16" stroke="white" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
          <h2 class="form-title">Sign in</h2>
          <p class="form-subtitle">Enter your credentials to access your account</p>
        </div>

        <!-- Alert -->
        <transition name="fade">
          <div v-if="errorMessage" class="alert alert--error" role="alert" aria-live="assertive">
            <svg class="alert__icon" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
              <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd"/>
            </svg>
            <span>{{ errorMessage }}</span>
          </div>
        </transition>

        <!-- Form -->
        <form class="login-form" @submit.prevent="handleSubmit" novalidate>
          <!-- Email -->
          <div class="field" :class="{ 'field--error': errors.email }">
            <label class="field__label" for="email">Email address</label>
            <div class="field__input-wrap">
              <svg class="field__icon" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                <path d="M2.003 5.884L10 9.882l7.997-3.998A2 2 0 0016 4H4a2 2 0 00-1.997 1.884z"/>
                <path d="M18 8.118l-8 4-8-4V14a2 2 0 002 2h12a2 2 0 002-2V8.118z"/>
              </svg>
              <input
                id="email"
                v-model.trim="form.email"
                type="email"
                class="field__input"
                placeholder="you@example.com"
                autocomplete="email"
                :aria-invalid="!!errors.email"
                :aria-describedby="errors.email ? 'email-error' : undefined"
                @blur="validateEmail"
              />
            </div>
            <p v-if="errors.email" id="email-error" class="field__error" role="alert">{{ errors.email }}</p>
          </div>

          <!-- Password -->
          <div class="field" :class="{ 'field--error': errors.password }">
            <div class="field__label-row">
              <label class="field__label" for="password">Password</label>
              <a href="#" class="field__forgot" tabindex="0">Forgot password?</a>
            </div>
            <div class="field__input-wrap">
              <svg class="field__icon" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                <path fill-rule="evenodd" d="M5 9V7a5 5 0 0110 0v2a2 2 0 012 2v5a2 2 0 01-2 2H5a2 2 0 01-2-2v-5a2 2 0 012-2zm8-2v2H7V7a3 3 0 016 0z" clip-rule="evenodd"/>
              </svg>
              <input
                id="password"
                v-model="form.password"
                :type="showPassword ? 'text' : 'password'"
                class="field__input field__input--password"
                placeholder="••••••••"
                autocomplete="current-password"
                :aria-invalid="!!errors.password"
                :aria-describedby="errors.password ? 'password-error' : undefined"
                @blur="validatePassword"
              />
              <button
                type="button"
                class="field__toggle"
                :aria-label="showPassword ? 'Hide password' : 'Show password'"
                @click="showPassword = !showPassword"
              >
                <svg v-if="!showPassword" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                  <path d="M10 12a2 2 0 100-4 2 2 0 000 4z"/>
                  <path fill-rule="evenodd" d="M.458 10C1.732 5.943 5.522 3 10 3s8.268 2.943 9.542 7c-1.274 4.057-5.064 7-9.542 7S1.732 14.057.458 10zM14 10a4 4 0 11-8 0 4 4 0 018 0z" clip-rule="evenodd"/>
                </svg>
                <svg v-else viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                  <path fill-rule="evenodd" d="M3.707 2.293a1 1 0 00-1.414 1.414l14 14a1 1 0 001.414-1.414l-1.473-1.473A10.014 10.014 0 0019.542 10C18.268 5.943 14.478 3 10 3a9.958 9.958 0 00-4.512 1.074l-1.78-1.781zm4.261 4.26l1.514 1.515a2.003 2.003 0 012.45 2.45l1.514 1.514a4 4 0 00-5.478-5.478z" clip-rule="evenodd"/>
                  <path d="M12.454 16.697L9.75 13.992a4 4 0 01-3.742-3.741L2.335 6.578A9.98 9.98 0 00.458 10c1.274 4.057 5.064 7 9.542 7 .847 0 1.669-.105 2.454-.303z"/>
                </svg>
              </button>
            </div>
            <p v-if="errors.password" id="password-error" class="field__error" role="alert">{{ errors.password }}</p>
          </div>

          <!-- Remember me -->
          <div class="field field--checkbox">
            <label class="checkbox-label">
              <input v-model="form.remember" type="checkbox" class="checkbox-input" />
              <span class="checkbox-custom" aria-hidden="true"></span>
              <span class="checkbox-text">Remember me for 30 days</span>
            </label>
          </div>

          <!-- Submit -->
          <button
            type="submit"
            class="btn-submit"
            :disabled="isLoading"
            :aria-busy="isLoading"
          >
            <span v-if="!isLoading">Sign in</span>
            <span v-else class="btn-submit__loading" aria-label="Signing in…">
              <svg class="spinner" viewBox="0 0 24 24" fill="none" aria-hidden="true">
                <circle cx="12" cy="12" r="10" stroke="currentColor" stroke-width="3" stroke-dasharray="31.4" stroke-dashoffset="10" stroke-linecap="round"/>
              </svg>
              Signing in…
            </span>
          </button>
        </form>

        <!-- Footer -->
        <p class="form-footer">
          Don't have an account?
          <RouterLink to="/register" class="form-footer__link">Create one free</RouterLink>
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { useRouter } from 'vue-router'
import { useAuthStore } from '@/stores/auth'

const router = useRouter()
const authStore = useAuthStore()

const form = reactive({ email: '', password: '', remember: false })
const errors = reactive({ email: '', password: '' })
const errorMessage = ref('')
const isLoading = ref(false)
const showPassword = ref(false)

const features = [
  { icon: '🔒', text: 'Enterprise-grade security' },
  { icon: '⚡', text: 'Lightning-fast performance' },
  { icon: '📊', text: 'Real-time analytics dashboard' },
]

function validateEmail() {
  if (!form.email) {
    errors.email = 'Email is required.'
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email)) {
    errors.email = 'Please enter a valid email address.'
  } else {
    errors.email = ''
  }
}

function validatePassword() {
  if (!form.password) {
    errors.password = 'Password is required.'
  } else if (form.password.length < 6) {
    errors.password = 'Password must be at least 6 characters.'
  } else {
    errors.password = ''
  }
}

function validate() {
  validateEmail()
  validatePassword()
  return !errors.email && !errors.password
}

async function handleSubmit() {
  errorMessage.value = ''
  if (!validate()) return

  isLoading.value = true
  try {
    await authStore.login({ email: form.email, password: form.password })
    router.push('/dashboard')
  } catch (err) {
    const msg = err?.response?.data?.message
    const emailErr = err?.response?.data?.errors?.email?.[0]
    errorMessage.value = emailErr || msg || 'Something went wrong. Please try again.'
  } finally {
    isLoading.value = false
  }
}
</script>

<style scoped>
/* ── Layout ─────────────────────────────────────────── */
.login-page {
  display: flex;
  min-height: 100vh;
  background: #f8fafc;
}

/* ── Brand panel ─────────────────────────────────────── */
.login-page__brand {
  position: relative;
  display: none;
  flex: 1;
  background: linear-gradient(135deg, #6366f1 0%, #8b5cf6 50%, #a855f7 100%);
  overflow: hidden;
  padding: 3rem;
  align-items: center;
  justify-content: center;
}

@media (min-width: 1024px) {
  .login-page__brand {
    display: flex;
  }
}

.brand-content {
  position: relative;
  z-index: 1;
  max-width: 420px;
  color: #fff;
}

.brand-logo {
  margin-bottom: 2rem;
}

.brand-title {
  font-size: 2.5rem;
  font-weight: 700;
  line-height: 1.2;
  margin: 0 0 1rem;
  letter-spacing: -0.02em;
}

.brand-subtitle {
  font-size: 1.05rem;
  line-height: 1.7;
  opacity: 0.85;
  margin: 0 0 2.5rem;
}

.brand-features {
  display: flex;
  flex-direction: column;
  gap: 0.85rem;
}

.feature-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  font-size: 0.95rem;
  opacity: 0.9;
}

.feature-icon {
  font-size: 1.1rem;
}

/* Decorative circles */
.brand-circles {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.circle {
  position: absolute;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.06);
}

.circle--1 {
  width: 400px;
  height: 400px;
  top: -100px;
  right: -100px;
}

.circle--2 {
  width: 300px;
  height: 300px;
  bottom: -80px;
  left: -80px;
}

.circle--3 {
  width: 200px;
  height: 200px;
  bottom: 30%;
  right: 10%;
  background: rgba(255, 255, 255, 0.04);
}

/* ── Form panel ──────────────────────────────────────── */
.login-page__form-panel {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem 1.5rem;
}

.form-wrapper {
  width: 100%;
  max-width: 420px;
}

/* ── Form header ─────────────────────────────────────── */
.form-header {
  margin-bottom: 2rem;
}

.form-logo {
  margin-bottom: 1.25rem;
}

.form-title {
  font-size: 1.75rem;
  font-weight: 700;
  color: #0f172a;
  margin: 0 0 0.4rem;
  letter-spacing: -0.02em;
}

.form-subtitle {
  font-size: 0.9rem;
  color: #64748b;
  margin: 0;
}

/* ── Alert ───────────────────────────────────────────── */
.alert {
  display: flex;
  align-items: flex-start;
  gap: 0.6rem;
  padding: 0.85rem 1rem;
  border-radius: 10px;
  font-size: 0.875rem;
  margin-bottom: 1.25rem;
}

.alert--error {
  background: #fef2f2;
  color: #b91c1c;
  border: 1px solid #fecaca;
}

.alert__icon {
  width: 1.1rem;
  height: 1.1rem;
  flex-shrink: 0;
  margin-top: 1px;
}

/* ── Fields ──────────────────────────────────────────── */
.login-form {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.field {
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}

.field__label-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.field__label {
  font-size: 0.875rem;
  font-weight: 500;
  color: #374151;
}

.field__forgot {
  font-size: 0.8rem;
  color: #6366f1;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.15s;
}

.field__forgot:hover {
  color: #4f46e5;
  text-decoration: underline;
}

.field__input-wrap {
  position: relative;
  display: flex;
  align-items: center;
}

.field__icon {
  position: absolute;
  left: 0.85rem;
  width: 1rem;
  height: 1rem;
  color: #9ca3af;
  pointer-events: none;
  flex-shrink: 0;
}

.field__input {
  width: 100%;
  padding: 0.7rem 0.9rem 0.7rem 2.5rem;
  border: 1.5px solid #e2e8f0;
  border-radius: 10px;
  font-size: 0.9rem;
  color: #0f172a;
  background: #fff;
  outline: none;
  transition: border-color 0.2s, box-shadow 0.2s;
  box-sizing: border-box;
}

.field__input::placeholder {
  color: #cbd5e1;
}

.field__input:focus {
  border-color: #6366f1;
  box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.12);
}

.field--error .field__input {
  border-color: #f87171;
}

.field--error .field__input:focus {
  box-shadow: 0 0 0 3px rgba(248, 113, 113, 0.15);
}

.field__input--password {
  padding-right: 2.75rem;
}

.field__toggle {
  position: absolute;
  right: 0.85rem;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  color: #9ca3af;
  display: flex;
  align-items: center;
  transition: color 0.15s;
}

.field__toggle:hover {
  color: #6366f1;
}

.field__toggle svg {
  width: 1.1rem;
  height: 1.1rem;
}

.field__error {
  font-size: 0.78rem;
  color: #dc2626;
  margin: 0;
}

/* ── Checkbox ────────────────────────────────────────── */
.field--checkbox {
  flex-direction: row;
  align-items: center;
}

.checkbox-label {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  cursor: pointer;
  user-select: none;
}

.checkbox-input {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}

.checkbox-custom {
  width: 1.1rem;
  height: 1.1rem;
  border: 1.5px solid #d1d5db;
  border-radius: 4px;
  background: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  transition: background 0.15s, border-color 0.15s;
}

.checkbox-input:checked + .checkbox-custom {
  background: #6366f1;
  border-color: #6366f1;
}

.checkbox-input:checked + .checkbox-custom::after {
  content: '';
  display: block;
  width: 5px;
  height: 9px;
  border: 2px solid #fff;
  border-top: none;
  border-left: none;
  transform: rotate(45deg) translateY(-1px);
}

.checkbox-input:focus-visible + .checkbox-custom {
  box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.25);
}

.checkbox-text {
  font-size: 0.875rem;
  color: #374151;
}

/* ── Submit button ───────────────────────────────────── */
.btn-submit {
  width: 100%;
  padding: 0.8rem;
  background: linear-gradient(135deg, #6366f1, #8b5cf6);
  color: #fff;
  border: none;
  border-radius: 10px;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  transition: opacity 0.2s, transform 0.1s, box-shadow 0.2s;
  box-shadow: 0 4px 14px rgba(99, 102, 241, 0.35);
  margin-top: 0.25rem;
}

.btn-submit:hover:not(:disabled) {
  opacity: 0.92;
  box-shadow: 0 6px 20px rgba(99, 102, 241, 0.45);
  transform: translateY(-1px);
}

.btn-submit:active:not(:disabled) {
  transform: translateY(0);
}

.btn-submit:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.btn-submit__loading {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

/* ── Spinner ─────────────────────────────────────────── */
.spinner {
  width: 1.1rem;
  height: 1.1rem;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

/* ── Footer ──────────────────────────────────────────── */
.form-footer {
  text-align: center;
  font-size: 0.875rem;
  color: #64748b;
  margin-top: 1.5rem;
}

.form-footer__link {
  color: #6366f1;
  font-weight: 600;
  text-decoration: none;
  transition: color 0.15s;
}

.form-footer__link:hover {
  color: #4f46e5;
  text-decoration: underline;
}

/* ── Transition ──────────────────────────────────────── */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.25s ease, transform 0.25s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(-6px);
}
</style>
