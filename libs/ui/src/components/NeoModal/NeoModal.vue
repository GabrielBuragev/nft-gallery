<template>
  <o-modal
    class="neo-modal"
    scroll="clip"
    :active.sync="isModalActive"
    :destroy-on-hide="destroyOnHide"
    :can-cancel="canCancel"
    :full-screen="fullScreen"
    :content-class="[...contentClassName, noShadow ? 'no-shadow' : '']"
    :root-class="rootClass"
    :style="{
      '--max-height': maxHeight,
    }"
    @close="updateClose">
    <slot />
  </o-modal>
</template>

<script lang="ts" setup>
import { OModal } from '@oruga-ui/oruga'

const props = withDefaults(
  defineProps<{
    value: boolean
    destroyOnHide?: boolean
    canCancel?: boolean | string[]
    fullScreen?: boolean
    contentClass?: string | string[]
    rootClass?: string
    noShadow?: boolean
    maxHeight?: string | number
  }>(),
  {
    destroyOnHide: true,
    canCancel: true,
    fullScreen: false,
    contentClass: '',
    rootClass: '',
    noShadow: false,
    maxHeight: '80vh',
  }
)

const emit = defineEmits(['close'])

const maxHeight = computed(() => {
  if (typeof props.maxHeight === 'number') {
    return `${props.maxHeight}px`
  }
  return props.maxHeight
})

const isModalActive = useVModel(props, 'value')

const contentClassName = computed(() => {
  if (Array.isArray(props.contentClass)) {
    return [...props.contentClass]
  }
  return [props.contentClass]
})
const updateClose = () => {
  emit('close', false)
}
</script>

<style lang="scss">
@import './NeoModal.scss';
</style>
