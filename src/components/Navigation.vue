
  <script lang="ts">
  import { defineComponent } from 'vue'
  export default defineComponent({
    name: 'Navigation'
  })
  </script>

<template>
  <nav class="navbar">
    <div class="navbar-container">
      <!-- Logo / Brand -->
      <a href="#" class="navbar-logo">Xavier Hua</a>
      
      <!-- Navigation Links -->
      <ul class="navbar-links">
        <li><a href="#about">About</a></li>
        <li><a href="#skills">Skills</a></li>
        <li><a href="#experience">Experience</a></li>
        <li><a href="#projects">Projects</a></li>
        <li><a href="#education">Education</a></li>
        <li><a href="#contact">Contact</a></li>
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
  box-shadow: 0 1px 2px rgba(0,0,0,0.05);
}

/* Inner container for spacing and layout */
.navbar-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0.75rem 1rem; /* Adjust as needed */
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 1.2rem;
}

/* LOGO */
.navbar-logo {
  background: linear-gradient(to right, #6366f1, #a855f7); 
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  transition: opacity 0.2s;
  font-size: 1.5rem;
}

.navbar-logo:hover {
  opacity: 0.8;
}

/* NAV LINKS */
.navbar-links {
  list-style: none;
  display: flex;
  gap: 1.5rem;
}

.navbar-links a {
  color: #374151; /* text-gray-700 */
  text-decoration: none;
  font-weight: 500;
  transition: color 0.2s;
}

.navbar-links a:hover {
  color: #111827; /* text-gray-900 */
}
</style>