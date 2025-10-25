<template>
  <div class="home-wrapper">
  <div class="circle"></div>
    <header class="hero">
      <div class="hero-content">
        <h1>Ticket Web App</h1>
        <p class="tagline">Manage your tickets effortlessly across teams and timelines.</p>
        <div class="hero-buttons">
          <router-link to="/login" class="btn">Login</router-link>
          <router-link to="/signup" class="btn secondary">Get Started</router-link>
        </div>
      </div>

      <img src="/hero-wave.svg" alt="Wave background" class="wave" />
    </header>

    <section class="features">
      <div class="feature-card">
        <h3>Secure Authentication</h3>
        <p>Your data is protected with industry-standard security.</p>
      </div>
      <div class="feature-card">
        <h3>Real-Time Ticket Updates</h3>
        <p>Stay in sync with your team and track progress instantly.</p>
      </div>
      <div class="feature-card">
        <h3>Responsive Design</h3>
        <p>Access your dashboard from any device, anytime.</p>
      </div>
    </section>

    <footer class="footer">
      © 2025 Ticket Web App. All rights reserved.
    </footer>
  </div>
</template>

<style scoped>
.home-wrapper {
  font-family: 'Inter', sans-serif;
  background: linear-gradient(to bottom right, #f0faff, #ffffff);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow-x: hidden;
}

.hero {
  position: relative;
  text-align: center;
  padding: 6rem 2rem 3rem;
  color: #1a2b4c;
  overflow: hidden;
}

.hero-content {
  position: relative;
  z-index: 2;
  max-width: 700px;
  margin: 0 auto;
}

.hero h1 {
  font-size: 3rem;
  font-weight: 700;
  margin-bottom: 1rem;
  color: #0077cc;
}

.tagline {
  font-size: 1.25rem;
  color: #444;
  margin-bottom: 2.5rem;
}

.hero-buttons {
  display: flex;
  gap: 1rem;
  justify-content: center;
  flex-wrap: wrap;
}

.circle {
  position: fixed;
  top: 20px;
  right: 20px;
  width: 120px;
  height: 120px;
  background-color: #0077cc;
  border-radius: 50%;
  opacity: 0.1;
  z-index: 100;
  pointer-events: none;
}

.btn {
  padding: 0.85rem 1.75rem;
  background-color: #0077cc;
  color: white;
  border-radius: 8px;
  text-decoration: none;
  font-weight: 600;
  font-size: 1rem;
  box-shadow: 0 4px 12px rgba(0, 119, 204, 0.2);
  transition: all 0.3s ease;
}

.btn:hover {
  background-color: #005fa3;
  transform: translateY(-2px);
}

.btn.secondary {
  background-color: #00b894;
  box-shadow: 0 4px 12px rgba(0, 184, 148, 0.2);
}

.btn.secondary:hover {
  background-color: #00997a;
  transform: translateY(-2px);
}

.wave {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  z-index: 1;
}

.features {
  display: flex;
  justify-content: center;
  gap: 2rem;
  flex-wrap: wrap;
  padding: 4rem 2rem;
  background-color: #f8fbff;
  border-top: 1px solid #e0eafc;
}

.feature-card {
  background-color: white;
  border-radius: 12px;
  padding: 2rem;
  max-width: 320px;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.05);
  text-align: center;
  transition: transform 0.3s ease;
}

.feature-card:hover {
  transform: translateY(-5px);
}

.feature-card h3 {
  margin-bottom: 0.75rem;
  color: #0077cc;
  font-size: 1.2rem;
}

.feature-card p {
  color: #555;
  font-size: 0.95rem;
}

.footer {
  margin-top: auto;
  padding: 1.5rem;
  text-align: center;
  font-size: 0.9rem;
  color: #888;
  background-color: #f0f4f8;
  width: 100%;
}
</style>
