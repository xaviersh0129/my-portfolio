<template>
  <section id="contact" class="contact-section">
    <div class="contact-container">
      <!-- Heading Row -->
      <div class="contact-header">
        <Mail class="contact-icon" />
        <h2 class="contact-title gradient-text">Get In Touch</h2>
      </div>

      <!-- Intro Card -->
      <div class="contact-content">
        <div class="contact-intro">
          <p>{{ contactData?.introText }}</p>
        </div>

        <!-- Social Links -->
        <div class="contact-links">
          <a
            v-for="(linkItem, i) in contactData?.links"
            :key="i"
            :href="linkItem.url"
            target="_blank"
            rel="noopener noreferrer"
            class="social-link"
          >
            <!-- Render the icon if it exists -->
            <component
              v-if="linkItem.icon"
              :is="getIconComponent(linkItem.icon)"
              class="w-6 h-6"
            />
            <!-- Display the link label -->
            <span>{{ linkItem.label }}</span>
          </a>
        </div>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue'
import { Mail, Linkedin, Github, Instagram } from 'lucide-vue-next'

export default defineComponent({
  name: 'ContactSection',
  components: { Mail },
  setup() {
    // Our contact data (intro text + links) loaded from JSON
    const contactData = ref<{
      introText: string
      links: { label: string; url: string; icon?: string }[]
    } | null>(null)

    // Icon map: match the 'icon' property in JSON to the actual Lucide component
    const iconMap: Record<string, any> = {
      linkedin: Linkedin,
      github: Github,
      instagram: Instagram
    }

    // Helper to retrieve the correct icon component
    const getIconComponent = (iconName: string) => {
      const key = iconName.toLowerCase()
      return iconMap[key] || null
    }

    onMounted(async () => {
      // Fetch the contact data from public/contact.json
      try {
        const response = await fetch('/contact.json')
        if (!response.ok) {
          throw new Error('Failed to fetch contact data')
        }
        contactData.value = await response.json()
      } catch (error) {
        console.error('Error loading contact data:', error)
      }

      // Intersection Observer for fade-in effect
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

      const contactSection = document.querySelector('.contact-section')
      if (contactSection) {
        (contactSection as HTMLElement).style.opacity = '0'
        observer.observe(contactSection)
      }
    })

    return { contactData, getIconComponent }
  }
})
</script>

<style scoped>
/* SECTION WRAPPER */
.contact-section {
  padding: 5rem 1rem; /* ~py-20, px-4 */
  background-color: #f9fafb; /* ~bg-gray-50 */
}

/* CONTAINER */
.contact-container {
  max-width: 72rem; /* ~max-w-6xl */
  margin: 0 auto;
}

/* HEADER */
.contact-header {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0rem;
}

.contact-icon {
  width: 2rem;
  height: 2rem;
  color: #2563eb; /* ~blue-600 */
}

.contact-title {
  font-size: 2.25rem; /* ~text-4xl */
  font-weight: 700;
}

/* MAIN CONTENT */
.contact-content {
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Intro card with white background */
.contact-intro {
  background-color: #fff;
  padding: 2rem;      /* ~p-8 */
  border-radius: 1rem; /* ~rounded-2xl */
  box-shadow: 0 1px 2px rgba(0,0,0,0.05); /* ~shadow-sm */
  max-width: 60rem; /* ~max-w-2xl */
  width: 100%;
  margin-bottom: 2rem; /* ~mb-8 */
  text-align: center;
}

.contact-intro p {
  font-size: 1.125rem; /* ~text-lg */
  color: #4b5563;      /* ~text-gray-600 */
  line-height: 1.6;
  margin: 0;
}

/* Social links row */
.contact-links {
  display: flex;
  gap: 2rem; /* ~gap-8 */
  justify-content: center;
}

/* Social link styling */
.social-link {
  display: flex;
  align-items: center;
  gap: 0.5rem; 
  color: #4B5563; /* text-gray-600 */
  transition: color 200ms, transform 200ms;
  text-decoration: none;
  font-weight: 500;
}
.social-link:hover {
  color: #2563eb; /* text-blue-600 */
  transform: scale(1.05);
}

/* GRADIENT TEXT */
.gradient-text {
  background: linear-gradient(to right, #2563eb, #7e22ce);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* FADE-IN ANIMATION */
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
</style>