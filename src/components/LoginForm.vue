<script setup>
import { ref } from 'vue'
import logoUrl from '../../github-3382.png'

const username = ref('')
const password = ref('')
const usernameError = ref('')
const passwordError = ref('')
const isSubmitting = ref(false)

const isValidUsername = (value) => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  const usernameRegex = /^[a-zA-Z0-9_-]{1,39}$/
  return emailRegex.test(value) || usernameRegex.test(value)
}

const clearErrors = () => {
  usernameError.value = ''
  passwordError.value = ''
}

const validateUsername = () => {
  if (!username.value.trim()) {
    usernameError.value = 'Username or email is required'
    return false
  }

  if (!isValidUsername(username.value)) {
    usernameError.value = 'Please enter a valid email or username'
    return false
  }

  usernameError.value = ''
  return true
}

const validatePassword = () => {
  if (!password.value) {
    passwordError.value = 'Password is required'
    return false
  }

  if (password.value.length < 6) {
    passwordError.value = 'Password should be at least 6 characters'
    return false
  }

  passwordError.value = ''
  return true
}

const handleSignIn = () => {
  if (isSubmitting.value) {
    return
  }

  clearErrors()
  const usernameValid = validateUsername()
  const passwordValid = validatePassword()

  if (!usernameValid || !passwordValid) {
    return
  }

  isSubmitting.value = true
  setTimeout(() => {
    alert(`Welcome, ${username.value}!\n\nThis is a demo page. Sign in successful!`)
    username.value = ''
    password.value = ''
    isSubmitting.value = false
  }, 1000)
}
</script>

<template>
  <div class="container">
    <div class="login-box">
      <div class="logo-section">
        <img :src="logoUrl" alt="GitHub Logo" class="logo" />
      </div>

      <h1>Sign in to GitHub</h1>

      <form id="loginForm" @submit.prevent="handleSignIn" novalidate>
        <div class="form-group">
          <label for="username">Username or email address</label>
          <input
            id="username"
            name="username"
            type="text"
            placeholder="Username or email address"
            v-model="username"
            :class="{ error: usernameError }"
            @blur="validateUsername"
            required
          />
          <span class="error-message" v-if="usernameError">{{ usernameError }}</span>
        </div>

        <div class="form-group">
          <div class="form-row">
            <label for="password">Password</label>
            <a href="#" class="link">Forgot password?</a>
          </div>
          <input
            id="password"
            name="password"
            type="password"
            placeholder="Password"
            v-model="password"
            :class="{ error: passwordError }"
            @blur="validatePassword"
            required
          />
          <span class="error-message" v-if="passwordError">{{ passwordError }}</span>
        </div>

        <button type="submit" class="signin-btn" :disabled="isSubmitting">
          {{ isSubmitting ? 'Signing in...' : 'Sign in' }}
        </button>
      </form>

      <div class="signup-section">
        <p>New to GitHub? <a href="#" class="link signup-link">Create an account</a></p>
      </div>
    </div>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 16px;
  background-color: #f6f8fa;
}

.login-box {
  width: 100%;
  max-width: 340px;
  padding: 40px;
  background-color: #f6f8fa;
  border: 1px solid transparent;
  border-radius: 6px;
}

.logo-section {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.logo {
  width: 44px;
  height: 44px;
}

h1 {
  font-size: 24px;
  font-weight: 600;
  text-align: center;
  margin-bottom: 32px;
  color: #24292e;
}

.form-group {
  margin-bottom: 16px;
  display: flex;
  flex-direction: column;
}

label {
  font-size: 14px;
  font-weight: 500;
  margin-bottom: 6px;
  display: block;
}

input[type='text'],
input[type='password'] {
  padding: 10px 12px;
  font-size: 14px;
  border: 1px solid #e1e4e8;
  border-radius: 6px;
  background-color: #fafbfc;
  color: #24292e;
  transition: border-color 0.2s, background-color 0.2s;
}

input[type='text']:focus,
input[type='password']:focus {
  outline: none;
  border-color: #0969da;
  background-color: #ffffff;
  box-shadow: 0 0 0 3px rgba(9, 105, 218, 0.14);
}

input::placeholder {
  color: #6a737d;
}

.error-message {
  color: #cb2431;
  font-size: 12px;
  margin-top: 4px;
}

input.error {
  border-color: #cb2431;
}

.signin-btn {
  width: 100%;
  padding: 10px 16px;
  margin-top: 20px;
  font-size: 14px;
  font-weight: 600;
  color: white;
  background-color: #1f883d;
  border: 1px solid rgba(27, 31, 35, 0.15);
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.2s, border-color 0.2s;
}

.signin-btn:hover {
  background-color: #19682d;
}

.signin-btn:active {
  background-color: #165d2a;
}

.signin-btn:disabled {
  background-color: #6c757d;
  cursor: not-allowed;
}

.link {
  color: #0969da;
  text-decoration: none;
  font-size: 13px;
  transition: color 0.2s;
}

.link:hover {
  color: #0256c7;
  text-decoration: underline;
}

.signup-section {
  border-top: 1px solid #e1e4e8;
  margin-top: 20px;
  padding-top: 20px;
  text-align: center;
  font-size: 13px;
}

.form-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
  margin-bottom: 8px;
}

.form-row label {
  margin-bottom: 0;
}

.signup-section p {
  color: #586069;
}

.signup-link {
  color: #0366d6;
}

@media (max-width: 576px) {
  .login-box {
    padding: 24px;
  }

  h1 {
    font-size: 20px;
    margin-bottom: 24px;
  }

  .logo {
    width: 36px;
    height: 36px;
  }
}
</style>
