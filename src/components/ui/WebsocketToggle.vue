<template>
  <div
    class="ws-toggle animate-glow"
    @click="toggle"
    :title="isOnline ? 'Se déconnecter' : 'Se connecter'"
    role="switch"
    :aria-checked="isOnline"
    tabindex="0"
    @keyup.space.enter="toggle"
  >
    <div class="toggle-slider" :class="{ 'toggle-off': !isOnline }"></div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  modelValue: {
    type: Boolean,
    default: true,
  },
})
const emit = defineEmits(['update:modelValue', 'change'])
const isOnline = ref(props.modelValue)

watch(
  () => props.modelValue,
  (val) => {
    isOnline.value = val
  },
)

function toggle() {
  isOnline.value = !isOnline.value
  emit('update:modelValue', isOnline.value)
  emit('change', isOnline.value)
}
</script>

<style lang="sass" scoped>
.ws-toggle
  width: 60px
  height: 30px
  background: rgba(255, 255, 255, 0.2)
  border-radius: 20px
  position: relative
  cursor: pointer
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1)
  backdrop-filter: blur(10px)
  border: 1px solid rgba(255, 255, 255, 0.3)
  display: flex
  align-items: center

  &:hover
    background: rgba(255, 255, 255, 0.25)
    transform: translateY(-1px)
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15)

  .toggle-slider
    position: absolute
    width: 26px
    height: 26px
    background: white
    border-radius: 50%
    top: 2px
    left: 2px
    transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1), background 0.3s
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2)
    display: flex
    align-items: center
    justify-content: center

    // Icône check verte
    &::before
      content: '✔️'
      position: absolute
      left: 50%
      top: 50%
      transform: translate(-50%, -50%)
      font-size: 16px
      color: #4CAF50

    // état OFF : translate, fond sombre, croix rouge
    &.toggle-off
      transform: translateX(30px)
      background: #333
      &::before
        content: '❌'
        color: #FF5252

@keyframes glow
  0%, 100%
    box-shadow: 0 0 5px rgba(76, 175, 80, 0.22)
  50%
    box-shadow: 0 0 20px rgba(76, 175, 80, 0.32)

.animate-glow
  animation: glow 2s ease-in-out infinite

.ws-toggle
  &:active
    .toggle-slider
      transform: scale(0.95) translateX(var(--translate-x, 0))
  animation: slideIn 0.5s ease-out

@keyframes slideIn
  from
    transform: translateX(100px)
    opacity: 0
  to
    transform: translateX(0)
    opacity: 1
</style>
