<template>
  <section id="education" class="education-section">
    <div class="education-container">
      <!-- Header Row -->
      <div class="education-header">
        <GraduationCap class="education-icon" />
        <h2 class="education-title">Education</h2>
      </div>

      <!-- Render each education item from the JSON file -->
      <div
        v-for="(item, index) in educationList"
        :key="index"
        class="education-card card-hover"
      >
        <!-- University Image -->
        <img
          :src="item.image"
          :alt="item.imageAlt"
          class="education-image"
        />

        <!-- Text Content -->
        <div class="education-details">
          <h3 class="education-university">
            {{ item.university }}
          </h3>
          <p class="education-degree">
            {{ item.degree }}
          </p>
          <p class="education-minor">
            Minor in {{ item.minor }}
          </p>
          <p class="education-gpa">
            GPA: {{ item.gpa }}
          </p>
        </div>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue'
import { GraduationCap } from 'lucide-vue-next'

export default defineComponent({
  name: 'EducationSection',
  components: { GraduationCap },
  setup() {
    const educationList = ref<any[]>([])

    onMounted(async () => {
      try {
        // Fetch the JSON from public/education.json
        const response = await fetch('./education.json')
        if (!response.ok) {
          throw new Error('Failed to fetch education data')
        }
        // Store the array in educationList
        educationList.value = await response.json()
      } catch (error) {
        console.error('Error loading education data:', error)
      }

      // Optional: Intersection Observer for fade-in effect
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

      // Observe each card
      const cards = document.querySelectorAll('.card-hover')
      cards.forEach(card => {
        (card as HTMLElement).style.opacity = '0'
        observer.observe(card)
      })
    })

    return { educationList }
  }
})
</script>

<style scoped>
/* SECTION & CONTAINER */
.education-section {
  padding: 80px 16px; /* ~py-20 px-4 */
  background-color: #ffffff;
}

.education-container {
  max-width: 1152px; /* ~max-w-6xl */
  margin: 0 auto;
}

/* HEADER */
.education-header {
  display: flex;
  align-items: center;
  gap: 8px;  /* ~gap-2 */
  margin-bottom: 0rem; /* ~mb-8 */
}

.education-icon {
  width: 32px;
  height: 32px;
  color: #2563eb; /* ~text-blue-600 */
}

.education-title {
  font-size: 36px; /* ~text-4xl */
  font-weight: 700;
  background: linear-gradient(to right, #2563eb, #7e22ce);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* CARD LAYOUT */
.education-card {
  display: flex;
  align-items: center;
  background-color: #f9fafb; /* ~gray-50 */
  padding: 24px;  /* ~p-6 */
  border-radius: 16px; /* ~rounded-2xl */
  box-shadow: 0 1px 2px rgba(0,0,0,0.05); /* ~shadow-sm */
  margin-bottom: 32px; /* ~mb-8 */
  position: relative;
  transition: all 0.3s ease;
}

/* Hover effect */
.card-hover:hover {
  box-shadow: 0 10px 15px rgba(0,0,0,0.1);
  transform: translateY(-4px);
}

/* IMAGE & TEXT */
.education-image {
  width: 96px;
  height: 96px;
  border-radius: 8px; /* ~rounded-lg */
  object-fit: cover;
  margin-right: 24px;
}

.education-details {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.education-university {
  font-size: 24px;  /* ~text-2xl */
  font-weight: 700; /* ~font-bold */
  color: #111827;   /* ~text-gray-900 */
  margin: 0;
}

.education-degree {
  color: #2563eb; /* ~text-blue-600 */
  font-size: 18px; /* ~text-lg */
  margin: 0;
}

.education-minor,
.education-gpa {
  color: #4b5563; /* ~text-gray-600 */
  font-size: 16px; /* ~text-base */
  margin: 0;
  font-weight: 500;
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

@media (max-width: 600px) {
  /* Adjust education section padding */
  .education-section {
    padding: 40px 8px;
  }

  /* Adjust header layout for smaller screens */
  .education-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 4px;
  }

  /* Scale down the education title font size */
  .education-title {
    font-size: 28px;
  }

  /* Stack education card content vertically */
  .education-card {
    flex-direction: column;
    align-items: center;
    text-align: center;
  }

  /* Adjust image size and spacing */
  .education-image {
    width: 80px;
    height: 80px;
    margin-right: 0;
    margin-bottom: 12px;
  }

  /* Center align education details */
  .education-details {
    align-items: center;
  }
}
</style>