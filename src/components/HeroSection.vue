<template>
  <section class="hero-section" id="home">
    <div class="hero-container">
      <!-- Profile Circle + Icon -->
      <div class="profile-wrapper">
        <img 
          :src="heroData?.profileImage" 
          alt="Profile"
          class="profile-img"
        />
        <div class="code-icon">
          <span>&lt;/&gt;</span>
        </div>
      </div>

      <!-- Name / Title -->
      <h1 class="hero-title">
        <span class="gradient-text">{{ heroData?.name }}</span>
      </h1>

      <!-- Subtitle -->
      <p class="hero-subtitle">
        {{ heroData?.subtitle }}
      </p>

      <!-- Call to Action Buttons -->
      <div class="hero-buttons">
        <a href="#contact" class="btn btn-primary">Contact Me</a>
        <a href="#projects" class="btn btn-outline">View Projects</a>
      </div>
    </div>
    <!-- Downward Arrow -->
    <a href="#about" class="arrow-link">
      <!-- Basic Down Arrow SVG -->
      <svg
        width="32"
        height="32"
        fill="currentColor"
        viewBox="0 0 24 24"
      >
        <path d="M12 13.172l4.95-4.95 1.414 1.414L12 16 5.636 9.636l1.414-1.414z"></path>
      </svg>
    </a>
  </section>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue'

export default defineComponent({
  name: 'HeroSection',
  setup() {
    const heroData = ref<{
      profileImage: string;
      name: string;
      subtitle: string;
    } | null>(null)

    onMounted(async () => {
      try {
        // Fetch the hero data from public/hero.json
        const response = await fetch('/hero.json')
        if (!response.ok) {
          throw new Error('Failed to fetch hero data')
        }
        heroData.value = await response.json()
      } catch (error) {
        console.error('Error loading hero data:', error)
      }
    })

    return { heroData }
  }
})
</script>

<style scoped>
/* HERO SECTION WRAPPER */
.hero-section {
  min-height: 95vh;
  background-color: #fafafa;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  padding: 0 1rem;
  position: relative;
}

/* CONTAINER */
.hero-container {
  max-width: 900px;
  margin: 0 auto;
  position: relative;
}

/* PROFILE WRAPPER */
.profile-wrapper {
  position: relative;
  display: inline-block;
  margin-bottom: 0rem;
}

/* PROFILE IMAGE */
.profile-img {
  width: 180px;
  height: 180px;
  border-radius: 50%;
  object-fit: cover;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}

/* CODE ICON */
.code-icon {
  position: absolute;
  right: -0.75rem;
  bottom: -0.75rem;
  background-color: #6366f1;
  color: #fff;
  width: 2.25rem;
  height: 2.25rem;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 8px rgba(99,102,241,0.3);
}

/* HERO TITLE */
.hero-title {
  font-size: 2rem;
  font-weight: 700;
  color: #111827;
  margin-bottom: 1rem;
}

/* Gradient text */
.gradient-text {
  background: linear-gradient(to right, #6366f1, #a855f7);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  font-size: 3.5rem;
}

/* SUBTITLE */
.hero-subtitle {
  font-size: 1.125rem;
  color: #6b7280;
  max-width: 40rem;
  margin: 0 auto 2rem;
  line-height: 1.5;
}

/* CTA BUTTONS */
.hero-buttons {
  display: flex;
  gap: 1rem;
  justify-content: center;
}

/* BUTTON STYLES */
.btn {
  text-decoration: none;
  font-weight: 500;
  padding: 0.75rem 1.5rem;
  border-radius: 0.5rem;
  transition: transform 0.2s, box-shadow 0.2s;
}

.btn-primary {
  background: linear-gradient(to right, #6366f1, #a855f7);
  color: #fff;
}

.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 16px rgba(99,102,241,0.3);
}

.btn-outline {
  border: 2px solid #e5e7eb;
  color: #374151;
  background-color: #fff;
}

.btn-outline:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 16px rgba(0,0,0,0.1);
}

/* DOWNWARD ARROW */
.arrow-link {
  position: absolute;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
  color: #a855f7;
  animation: arrow-bounce 2s infinite;
  text-decoration: none;
}

.arrow-link:hover {
  opacity: 0.8;
}

@keyframes arrow-bounce {
  0%, 20%, 50%, 80%, 100% {
    transform: translate(-50%, 0);
  }
  40% {
    transform: translate(-50%, -10px);
  }
  60% {
    transform: translate(-50%, -5px);
  }
}
</style>