<script setup>
import { isEqual } from 'lodash'
import ToggleSwitch from '../components/ToggleSwitch.vue'
import IconArrow from '../components/icons/IconArrow.vue'

const props = defineProps({
  sections: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['update:sections'])

function toggleSection(id) {
  const sections = [...props.sections]
  const section = sections.find((section) => section.id === id)

  if (section) {
    section.visible = !section.visible
    emit('update:sections', sections)
  }
}

function toggleOption({ option, childName, sectionId }) {
  const sections = [...props.sections]
  const section = sections.find((section) => section.id === sectionId)

  if (section) {
    const child = section.children.find((child) => child.name === childName)

    if (!child.selected.includes(option)) {
      child.selected.push(option)
    } else {
      child.selected = child.selected.filter((selectedOption) => selectedOption != option)
    }

    emit('update:sections', sections)
  }
}

function getSwitchState({ options, selected }) {
  return isEqual(options.sort(), selected.sort())
}

function changeSelectedOptions({ state, childName, sectionId }) {
  const sections = [...props.sections]
  const section = sections.find((section) => section.id === sectionId)

  if (section) {
    const child = section.children.find((child) => child.name === childName)

    if (state) {
      child.selected = child.options
    } else {
      child.selected = []
    }

    emit('update:sections', sections)
  }
}
</script>

<template>
  <div>
    <div v-for="(section, index) of sections" :key="`section-${index}`" class="space-y-2">
      <div class="flex justify-between py-4">
        <p class="font-semibold">{{ section.name }}</p>

        <button type="button" @click="toggleSection(section.id)">
          <icon-arrow class="w-5 duration-200" :class="{ 'rotate-90': section.visible }" />
        </button>
      </div>
      <template v-if="section.visible">
        <div
          v-for="(child, index) of section.children"
          :key="`section-${index}-${child.name}`"
          class="p-4 border border-gray-200 rounded-md"
        >
          <div class="flex justify-between mb-2">
            <p class="font-semibold">{{ child.name }}</p>

            <toggle-switch
              :checked="getSwitchState(child)"
              @update:checked="
                changeSelectedOptions({
                  state: $event,
                  childName: child.name,
                  sectionId: section.id
                })
              "
            />
          </div>

          <div class="grid grid-cols-2 gap-2.5">
            <button
              v-for="(option, index) of child.options"
              :key="`section-${index}-${child.name}-${option}`"
              class="rounded-md p-2.5"
              :class="
                child.selected.includes(option)
                  ? 'bg-green-100 text-green-500'
                  : 'bg-gray-100 text-gray-500'
              "
              type="button"
              @click="toggleOption({ option, childName: child.name, sectionId: section.id })"
            >
              {{ option }}
            </button>
          </div>
        </div>
      </template>
    </div>
  </div>
</template>
