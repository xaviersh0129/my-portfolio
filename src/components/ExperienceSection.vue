<template>
  <section id="experience" class="experience-section">
    <div class="experience-container">
      <!-- Header -->
      <div class="experience-header">
        <Briefcase class="experience-icon" />
        <h2 class="experience-title">Work Experience</h2>
      </div>

      <!-- Experience Cards -->
      <div class="experience-cards">
        <div
          v-for="(job, index) in displayedJobs"
          :key="index"
          class="experience-card"
        >
          <h3 class="experience-card-title">{{ job.title }}</h3>
          <p class="experience-card-subtitle">
            {{ job.company }} | {{ job.period }}
          </p>
          <ul class="experience-list">
            <li v-for="(achievement, i) in job.achievements" :key="i">
              {{ achievement }}
            </li>
          </ul>
        </div>
      </div>
      <div v-if="jobs.length > 4" class="show-more-container" style="text-align: center; margin-top: 1rem;">
        <button @click="toggleShowExperiences" class="show-more-button" style="padding: 1rem 2rem; font-size: 1.25rem; border: none; background-color: #6366f1; color: #fff; border-radius: 0.5rem; cursor: pointer;">
          {{ showAllExperiences ? 'Show Less' : 'Show More' }}
        </button>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent, ref, computed, onMounted } from 'vue'
import { Briefcase } from 'lucide-vue-next'

export default defineComponent({
  name: 'ExperienceSection',
  components: { Briefcase },
  setup() {
    const jobs = ref<any[]>([])
    const showAllExperiences = ref(false)

    const displayedJobs = computed(() => {
      return showAllExperiences.value ? jobs.value : jobs.value.slice(0, 4)
    })

    const toggleShowExperiences = () => {
      showAllExperiences.value = !showAllExperiences.value
    }

    onMounted(async () => {
      try {
        const response = await fetch('./experience.json')
        if (!response.ok) {
          throw new Error('Failed to fetch experience data')
        }
        jobs.value = await response.json()
      } catch (error) {
        console.error('Error loading experience data:', error)
      }
    })

    return { jobs, showAllExperiences, displayedJobs, toggleShowExperiences }
  }
})
</script>

<style scoped>
/* Section styling (matches your React version's spacing/colors) */
.experience-section {
  padding: 5rem 1rem; /* ~py-20, px-4 */
  background-color: #fff;
}

.experience-container {
  max-width: 72rem; /* ~6xl */
  margin: 0 auto;
}

/* Header row (icon + gradient title) */
.experience-header {
  display: flex;
  align-items: center;
  gap: 0.5rem; /* ~gap-2 */
  margin-bottom: 0rem; /* ~mb-8 */
}

.experience-icon {
  width: 2rem;
  height: 2rem;
  color: #2563eb; /* ~blue-600 */
}

.experience-title {
  font-size: 2.25rem; /* ~text-4xl */
  font-weight: 700;
  background: linear-gradient(to right, #6366f1, #a855f7);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* Cards container */
.experience-cards {
  display: flex;
  flex-direction: column;
  gap: 2rem; /* ~space-y-8 */
}

/* Each experience card */
.experience-card {
  background-color: #f9fafb; /* ~gray-50 */
  padding: 1rem; /* ~p-8 */
  border-radius: 1rem; /* ~rounded-2xl */
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05); /* ~shadow-sm */
  transition: transform 0.3s, box-shadow 0.3s; /* ~card-hover */
}
.experience-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
}

/* Card heading and subtitle */
.experience-card-title {
  font-size: 1.5rem; /* ~text-2xl */
  font-weight: 700;  /* ~font-bold */
  color: #111827;    /* ~text-gray-900 */
  margin-bottom: 0.5rem;
}
.experience-card-subtitle {
  color: #2563eb;    /* ~blue-600 */
  margin-bottom: 1rem;
}

/* Achievements list */
.experience-list {
  list-style-type: disc;
  padding-left: 1.5rem; /* ~list-inside */
  color: #4b5563;       /* ~text-gray-600 */
  line-height: 1.6;
}
.experience-list li {
  margin-bottom: 0.5rem;
}

@media (max-width: 600px) {
  /* Adjust experience section padding */
  .experience-section {
    padding: 3rem 1rem;
  }

  /* Adjust header layout for smaller screens */
  .experience-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.25rem;
  }

  /* Scale down the experience title font size */
  .experience-title {
    font-size: 1.75rem;
  }

  /* Adjust experience card padding and font sizes */
  .experience-card {
    padding: 0.75rem;
  }
  .experience-card-title {
    font-size: 1.25rem;
  }
  .experience-card-subtitle {
    font-size: 1rem;
  }

  /* Adjust experience list font size */
  .experience-list {
    font-size: 0.9rem;
  }
}
</style>