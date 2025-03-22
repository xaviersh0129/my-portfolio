<template>
  <footer class="footer">
    <div class="footer-container">
      <p class="footer-text">© {{ currentYear }} Xavier Hua Shiao Hong. All rights reserved.</p>
    </div>
  </footer>
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue'
export default defineComponent({
  name: 'FooterComponent',
  setup() {
    const currentYear = computed(() => new Date().getFullYear())
    return { currentYear }
  }
})
</script>

<style scoped>
.footer {
  background-color: #ffffff; /* Replace with desired color */
  padding: 0rem 1rem;        /* Adjust spacing as needed */
}

/* Container is centered by setting max-width and margin: 0 auto */
.footer-container {
  max-width: 1200px;        /* Adjust to your preference */
  margin: 0 auto;
  text-align: center;       /* Centers the text content */
}

.footer-text {
  color: #666666;           /* Replace with desired text color */
}
</style>
