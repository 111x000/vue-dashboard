<script setup>
import SidePanelSections from '../components/SidePanelSections.vue'

defineProps({
  sections: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <aside class="hidden px-4 w-96 md:block">
    <div class="flex justify-center my-8">
      <img src="https://img.logoipsum.com/243.svg" alt="Main logo" />
    </div>
    <side-panel-sections :sections="sections" @update:sections="$emit('update:sections', $event)" />
  </aside>
</template>
