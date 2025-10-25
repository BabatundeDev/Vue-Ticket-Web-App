<template>
  <div class="signup-wrapper">
    <div class="signup-card">
      <h2>Sign Up</h2>
      <form @submit.prevent="handleSignup">
        <input v-model="email" type="email" placeholder="Email" />
        <input v-model="password" type="password" placeholder="Password" />
        <input v-model="confirmPassword" type="password" placeholder="Confirm Password" />
        <button type="submit">Sign Up</button>
      </form>
      <p v-if="error" class="error">{{ error }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
      confirmPassword: '',
      error: ''
    }
  },
  methods: {
    handleSignup() {
      if (!this.email || !this.password || !this.confirmPassword) {
        this.error = 'Please fill in all fields'
        return
      }
      if (this.password !== this.confirmPassword) {
        this.error = 'Passwords do not match'
        return
      }
      localStorage.setItem('user', this.email)
      this.$router.push('/dashboard')
    }
  }
}
</script>

<style scoped>
.signup-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(to bottom right, #e0f2ff, #ffffff);
  font-family: sans-serif;
}

.signup-card {
  background-color: white;
  padding: 2rem;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 400px;
  text-align: center;
}

h2 {
  margin-bottom: 1.5rem;
  color: #003366;
}

form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

input {
  padding: 0.75rem;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 1rem;
}

button {
  padding: 0.75rem;
  background-color: #0077cc;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #005fa3;
}

.error {
  color: red;
  margin-top: 1rem;
}
</style>
