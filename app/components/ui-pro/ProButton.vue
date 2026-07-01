<script setup lang="ts">
import type { PrimitiveProps } from 'reka-ui'
import type { HTMLAttributes } from 'vue'
import type { ButtonVariants } from '../ui/button'
import { reactiveOmit } from '@vueuse/core'

interface ButtonProps extends PrimitiveProps {
  variant?: ButtonVariants['variant']
  size?: ButtonVariants['size']
  class?: HTMLAttributes['class']
}

defineOptions({
  inheritAttrs: false,
})

const props = defineProps<Props>()

interface Props extends ButtonProps {
  loading?: boolean
}

const buttonProps = reactiveOmit(props, 'loading')

const attrs = useAttrs()
</script>

<template>
  <Button
    v-bind="{ ...attrs, ...buttonProps }"
  >
    <Spinner v-if="loading" class="animate-spin" />
    <slot />
  </Button>
</template>

<style scoped>

</style>
