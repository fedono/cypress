<template>
  <div class="min-w-full col-start-1 col-end-3 flex items-center gap-16px mb-24px relative">
    <Input
      id="project-search"
      v-model="localValue"
      name="project-search"
      type="search"
      class="min-w-200px w-85% flex-grow"
    />
    <label
      for="project-search"
      class="absolute text-gray-400 left-42px transition-opacity duration-50"
      :class="{'opacity-0': localValue.length}"
    >
      {{ t('globalPage.searchPlaceholder') }}
    </label>
    <Button
      aria-controls="dropzone"
      class="text-size-16px h-full"
      data-cy="addProjectButton"
      size="lg"
      :variant="showDropzone ? 'pending' : 'primary'"
      :aria-expanded="showDropzone"
      @click="toggleDropzone"
    >
      <template #prefix>
        <i-cy-add-large_x16
          class="transform duration-150"
          :class="showDropzone ?
            'icon-dark-gray-100 rotate-45' : 'icon-dark-indigo-300'"
        />
      </template>
      {{ t('globalPage.addProjectButton') }}
    </Button>
  </div>

  <FileDropzone
    v-if="showDropzone"
    id="dropzone"
    data-cy="dropzone"
    class="mb-24px"
    close-button
    @addProject="emit('addProject', $event)"
    @close="toggleDropzone"
  />

  <NoResults
    v-if="!projectCount"
    class="mt-80px"
    :search="localValue"
    :message="t('globalPage.noResultsMessage')"
    @clear="handleClear"
  />
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import Button from '@cy/components/Button.vue'
import Input from '@cy/components/Input.vue'
import FileDropzone from './FileDropzone.vue'
import { useModelWrapper } from '@packages/frontend-shared/src/composables'
import { useI18n } from '@cy/i18n'
import NoResults from '@cy/components/NoResults.vue'

const showDropzone = ref(false)
const { t } = useI18n()

const props = defineProps<{
  modelValue: string
  projectCount?: number | null
}>()

const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void
  (e: 'addProject', value: string | null): void
}>()

const localValue = useModelWrapper(props, emit, 'modelValue')

const handleClear = () => {
  localValue.value = ''
}

const toggleDropzone = () => {
  showDropzone.value = !showDropzone.value
}
</script>