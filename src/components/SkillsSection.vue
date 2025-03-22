<template>
  <section id="skills" class="skills-section">
    <div class="skills-container">
      <!-- Title & Icon -->
      <div class="skills-header">
        <Cpu class="skills-icon" />
        <h2 class="skills-title">Skills &amp; Technologies</h2>
      </div>

      <!-- Grid of Skill Cards -->
      <div class="skills-grid">
        <div
          v-for="skill in displayedSkills"
          :key="skill"
          class="skill-card"
        >
          <p class="skill-text">{{ skill }}</p>
        </div>
      </div>
      <div v-if="skills.length > 8" class="show-more-container" style="text-align: center; margin-top: 1rem;">
        <button @click="toggleShowSkills" class="show-more-button" style="padding: 1rem 2rem; font-size: 1.25rem; border: none; background-color: #6366f1; color: #fff; border-radius: 0.5rem; cursor: pointer;">
          {{ showAllSkills ? 'Show Less' : 'Show More' }}
        </button>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent, ref, computed, onMounted } from 'vue'
import { Cpu } from 'lucide-vue-next'

export default defineComponent({
  name: 'SkillsSection',
  components: { Cpu },
  setup() {
    const skills = ref<string[]>([])
    const showAllSkills = ref(false)

    const displayedSkills = computed(() => {
      return showAllSkills.value ? skills.value : skills.value.slice(0, 8)
    })

    const toggleShowSkills = () => {
      showAllSkills.value = !showAllSkills.value
    }

    onMounted(async () => {
      try {
        const response = await fetch('./skills.json')
        if (!response.ok) {
          throw new Error('Failed to fetch skills list')
        }
        skills.value = await response.json()
      } catch (error) {
        console.error(error)
      }
    })

    return { skills, showAllSkills, displayedSkills, toggleShowSkills }
  }
})
</script>

<style scoped>
/* Section Wrapper */
.skills-section {
  padding: 5rem 1rem; /* ~py-20, px-4 */
  background-color: #f9fafb; /* ~gray-50 */
}

/* Container */
.skills-container {
  max-width: 72rem; /* ~6xl */
  margin: 0 auto;
}

/* Header (icon + title) */
.skills-header {
  display: flex;
  align-items: center;
  gap: 0.5rem; /* ~gap-2 */
  margin-bottom: 0rem; /* ~mb-8 */
}

.skills-icon {
  width: 2rem;
  height: 2rem;
  color: #2563eb; /* ~blue-600 */
}

/* Gradient text title */
.skills-title {
  font-size: 2.25rem; /* ~text-4xl */
  font-weight: 700;
  background: linear-gradient(to right, #6366f1, #a855f7);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* Skills Grid */
.skills-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem; /* ~gap-6 */
}
@media (min-width: 768px) {
  .skills-grid {
    grid-template-columns: repeat(4, 1fr);
  }
}

/* Card styles */
.skill-card {
  background-color: #fff;
  padding: 0.5rem; /* ~p-6 */
  border-radius: 0.75rem; /* ~rounded-xl */
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05); /* ~shadow-sm */
  transition: transform 0.2s, box-shadow 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
}

.skill-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.08);
}

/* Text inside each card */
.skill-text {
  font-size: 1rem;  /* ~text-base */
  font-weight: 500; /* ~font-medium */
  color: #1f2937;   /* ~text-gray-800 */
  text-align: center;
}

@media (max-width: 600px) {
  /* Adjust skills section padding */
  .skills-section {
    padding: 3rem 1rem;
  }
  
  /* Stack header elements vertically */
  .skills-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.25rem;
  }
  
  /* Scale down skills title font size */
  .skills-title {
    font-size: 1.75rem;
  }
  
  /* Adjust skills grid to a single column layout */
  .skills-grid {
    grid-template-columns: 1fr;
  }
}
</style>