<template>
  <optgroup :label="label">
    <slot name="first" />
    <BFormSelectOption
      v-for="(option, index) in normalizedOptsWrapper"
      :key="index"
      :disabled="option.disabled"
      :value="option.value"
      v-bind="$attrs"
    >
      <!-- eslint-disable-next-line vue/no-v-html -->
      <span v-if="!!option.html" v-html="option.html" />
      <template v-else>
        {{ option.text }}
      </template>
    </BFormSelectOption>
    <slot />
  </optgroup>
</template>

<script setup lang="ts" generic="T = unknown">
import BFormSelectOption from './BFormSelectOption.vue'
import {useFormSelect} from '../../composables'
import type {SelectOption, SelectOptionRaw} from '../../types'
import {computed} from 'vue'

const props = withDefaults(
  defineProps<{
    label?: string
    options?: readonly SelectOptionRaw<T>[]
  }>(),
  {
    label: undefined,
    options: () => [],
  }
)

defineSlots<{
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  default?: (props: Record<string, never>) => any
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  first?: (props: Record<string, never>) => any
}>()

const {normalizedOptions} = useFormSelect(() => props.options)

const normalizedOptsWrapper = computed(() => normalizedOptions.value as SelectOption<T>[])
</script>
