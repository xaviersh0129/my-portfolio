<template>
  <section id="about" class="about-section">
    <div class="about-container">
      <!-- Title Row -->
      <div class="about-header">
        <User class="about-icon" />
        <h2 class="about-title">About Me</h2>
      </div>

      <!-- Text Box -->
      <div class="about-content-box">
        <!-- We'll render the loaded text via v-html for line breaks -->
        <p class="about-paragraph" v-html="aboutText"></p>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue'
import { User } from 'lucide-vue-next'

export default defineComponent({
  name: 'AboutSection',
  components: { User },
  setup() {
    const aboutText = ref('') // Holds the content from the text file

    onMounted(async () => {
      // Fetch the text from public/aboutContent.txt
      const response = await fetch('/aboutContent.txt')
      aboutText.value = await response.text()
    })

    return { aboutText }
  }
})
</script>

<style scoped>
.about-section {
  padding: 5rem 1rem;
  background-color: #fff;
}

.about-container {
  max-width: 72rem; /* ~6xl */
  margin: 0 auto;
}

.about-header {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0rem;
}

.about-icon {
  width: 2rem;
  height: 2rem;
  color: #2563eb; /* ~blue-600 */
}

.about-title {
  font-size: 2.25rem; /* ~text-4xl */
  font-weight: 700;
  background: linear-gradient(to right, #6366f1, #a855f7);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.about-content-box {
  background-color: #f9fafb; /* ~gray-50 */
  padding: 2rem; /* ~p-8 */
  border-radius: 1rem; /* ~rounded-2xl */
  box-shadow: 0 1px 2px rgba(0,0,0,0.05); /* ~shadow-sm */
}

.about-paragraph {
  font-size: 1.125rem; /* ~text-lg */
  color: #4b5563;      /* ~text-gray-600 */
  line-height: 1.75;   /* ~leading-relaxed */
  margin: 0;
}
</style>