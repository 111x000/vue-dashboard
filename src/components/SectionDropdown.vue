<script setup>
import { ref } from 'vue'
import IconArrow from '../components/icons/IconArrow.vue'

const visible = ref(true)
</script>

<template>
  <div class="border border-gray-200 rounded-md">
    <div class="flex justify-between p-3" :class="{ 'border-b': visible }">
      <h3 class="font-semibold">
        <slot name="title"></slot>
      </h3>

      <button type="button" @click="visible = !visible">
        <icon-arrow class="w-5 duration-200" :class="{ 'rotate-90': visible }" />
      </button>
    </div>
    <div v-if="visible" class="p-3">
      <slot name="content"></slot>
    </div>
  </div>
</template>
