<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'Navigation',
  setup() {
    const isOpen = ref(false)

    const toggleMenu = () => {
      isOpen.value = !isOpen.value
    }

    const closeMenu = () => {
      isOpen.value = false
    }

    return { isOpen, toggleMenu, closeMenu }
  }
})
</script>

<template>
  <nav class="navbar">
    <div class="navbar-container">
      <!-- Logo -->
      <a href="#" class="navbar-logo">Xavier Hua</a>

      <!-- Hamburger for mobile -->
      <button class="hamburger" @click="toggleMenu">
        <span :class="{ open: isOpen }"></span>
        <span :class="{ open: isOpen }"></span>
        <span :class="{ open: isOpen }"></span>
      </button>

      <!-- Nav Links -->
      <ul :class="['navbar-links', { show: isOpen }]">
        <li><a href="#about" @click="closeMenu">About</a></li>
        <li><a href="#skills" @click="closeMenu">Skills</a></li>
        <li><a href="#experience" @click="closeMenu">Experience</a></li>
        <li><a href="#projects" @click="closeMenu">Projects</a></li>
        <li><a href="#education" @click="closeMenu">Education</a></li>
        <li><a href="#contact" @click="closeMenu">Contact</a></li>
      </ul>
    </div>
  </nav>
</template>

<style scoped>
/* Basic Reset/Setup (optional) */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* NAVBAR CONTAINER */
.navbar {
  position: sticky;
  top: 0;
  z-index: 50;
  background-color: #fff;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
}

.navbar-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0.75rem 1rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.navbar-logo {
  font-size: 1.5rem;
  font-weight: 700;
  background: linear-gradient(to right, #6366f1, #a855f7);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* Nav links for desktop */
.navbar-links {
  list-style: none;
  display: flex;
  gap: 1.5rem;
}

.navbar-links a {
  text-decoration: none;
  color: #374151;
  font-weight: 500;
}

.navbar-links a:hover {
  color: #111827;
}

/* Hamburger menu */
.hamburger {
  display: none;
  flex-direction: column;
  gap: 5px;
  border: none;
  background: transparent;
  cursor: pointer;
}

.hamburger span {
  display: block;
  width: 25px;
  height: 3px;
  background: #6366f1;
  border-radius: 2px;
  transition: all 0.3s ease;
}

/* Mobile Styles */
@media (max-width: 768px) {
  .hamburger {
    display: flex;
  }

  .navbar-links {
    position: absolute;
    top: 64px;
    right: 0;
    background: white;
    flex-direction: column;
    align-items: flex-start;
    width: 100%;
    padding: 1rem 1.5rem;
    gap: 1rem;
    display: none;
  }

  .navbar-links.show {
    display: flex;
  }
}
</style>