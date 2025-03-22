<template>
  <section id="projects" class="projects-section">
    <div class="projects-container">
      <!-- Header row with icon + gradient title -->
      <div class="projects-header">
        <Code2 class="projects-icon" />
        <h2 class="projects-title">Featured Projects</h2>
      </div>

      <!-- Projects Grid -->
      <div class="projects-grid">
        <button
          v-for="(project, index) in projects"
          :key="index"
          class="project-card card-hover"
          type="button"
          @click="goToLink(project.link)"
        >
          <div class="project-image-wrapper">
            <img
              :src="project.image"
              :alt="project.title"
              class="project-image"
            />
            <div class="project-overlay"></div>
          </div>
          <div class="project-content">
            <h3 class="project-name">{{ project.title }}</h3>
            <p class="project-description">{{ project.description }}</p>
            <div class="project-tags">
              <span
                v-for="(tag, i) in project.tags"
                :key="i"
                class="skill-tag"
              >
                {{ tag }}
              </span>
            </div>
          </div>
        </button>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue'
import { Code2 } from 'lucide-vue-next'

export default defineComponent({
  name: 'ProjectsSection',
  components: { Code2 },
  setup() {
    const projects = ref<any[]>([])

    // Load the projects from JSON
    onMounted(async () => {
      try {
        const response = await fetch('/projects.json')
        if (!response.ok) {
          throw new Error('Failed to fetch projects data')
        }
        projects.value = await response.json()
      } catch (error) {
        console.error('Error loading projects:', error)
      }

      // Intersection Observer for fade-in
      const observerOptions = {
        threshold: 0.1,
        rootMargin: '0px 0px -50px 0px'
      }
      const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            entry.target.classList.add('animate-fade-in')
            observer.unobserve(entry.target)
          }
        })
      }, observerOptions)

      const cards = document.querySelectorAll('.project-card')
      cards.forEach(card => {
        (card as HTMLElement).style.opacity = '0'
        observer.observe(card)
      })
    })

    // Method to open link in new tab
    const goToLink = (link: string | undefined) => {
      if (link) {
        window.open(link, '_blank')
      }
    }

    return { projects, goToLink }
  }
})
</script>

<style scoped>
/* 1. Section Wrapper */
.projects-section {
  padding: 5rem 1rem; /* ~py-20, px-4 */
  background-color: #f9fafb; /* ~bg-gray-50 */
}

.projects-container {
  max-width: 72rem; /* ~max-w-6xl */
  margin: 0 auto;
}

/* 2. Header (icon + gradient title) */
.projects-header {
  display: flex;
  align-items: center;
  gap: 0.5rem; /* ~gap-2 */
  margin-bottom: 0rem; /* Adjust as needed */
}

.projects-icon {
  width: 2rem;
  height: 2rem;
  color: #6366f1; /* ~indigo-500 or your brand color */
}

.projects-title {
  font-size: 2.25rem; /* ~text-4xl */
  font-weight: 700;
  background: linear-gradient(to right, #6366f1, #a855f7);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* 3. Projects Grid */
.projects-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem;

  /* Make each row auto-size to the tallest item in that row */
  align-items: stretch; 
}

@media (min-width: 768px) {
  .projects-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* 4. Project Card (now a button) */
.project-card {
  display: flex;         /* So we can control layout inside */
  flex-direction: column; /* Image on top, content below */
  border: none;
  background-color: #fff; 
  border-radius: 1rem;
  overflow: hidden;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
  transition: transform 0.3s, box-shadow 0.3s;
  position: relative;
  cursor: pointer;
  text-align: left;
  padding: 0;
}

/* Remove default focus outline on button (optional) */
.project-card:focus {
  outline: none;
}

/* Hover effect */
.project-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
}

/* Project image + overlay */
/* Keep the fixed height for the image if you want a uniform image area */
.project-image-wrapper {
  overflow: hidden;
  width: 100%;
  height: 14rem;
  position: relative;
  flex: 0 0 auto; /* Don’t stretch the image wrapper vertically */
}

.project-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.project-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.5), transparent);
}

/* Card content area */
/* Let the text container stretch to fill remaining space */
.project-content {
  flex: 1 1 auto;
  padding: 1.5rem;
}

.project-name {
  font-size: 1.5rem; 
  font-weight: 700;
  color: #111827; 
  margin-bottom: 0.75rem;
}

.project-description {
  color: #4b5563; 
  margin-bottom: 1rem;
  font-size: 1rem;
  line-height: 1.5;
}

.project-tags {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
}

/* 5. Animate-Fade-In */
@keyframes fadeIn {
  0% {
    opacity: 0;
    transform: translateY(20px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-fade-in {
  animation: fadeIn 0.6s ease-out forwards;
}

/* Example skill-tag class */
.skill-tag {
  background-color: #eff6ff; 
  color: #2563eb;            
  padding: 0.25rem 0.75rem;
  border-radius: 9999px;
  font-size: 0.875rem;
  font-weight: 500;
  transition: background-color 0.2s;
}

.skill-tag:hover {
  background-color: #dbeafe; 
}
</style>