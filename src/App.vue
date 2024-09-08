<!-- App.vue -->
<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center">
    <!-- Use the Invoice component here -->
    <Invoice />
  </div>
</template>

<script>
import Invoice from './components/Invoice.vue';  // Import the Invoice component

export default {
  name: 'App',
  components: {
    Invoice,  // Register the Invoice component
  },
};
</script>

<style>
/* Optionally, you can add global styles here */
</style>