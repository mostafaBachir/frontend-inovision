<template>
  <div
    class="theme-toggle animate-glow"
    @click="toggleTheme"
    :title="$q.dark.isActive ? 'Passer en mode jour' : 'Passer en mode nuit'"
    role="button"
    tabindex="0"
    @keyup.space.enter="toggleTheme"
  >
    <div class="toggle-slider" :class="{ 'toggle-light': !$q.dark.isActive }"></div>
  </div>
</template>

<script setup>
import { useQuasar } from 'quasar'
const $q = useQuasar()

function toggleTheme() {
  $q.dark.toggle()
}
</script>

<style lang="sass" scoped>
.theme-toggle
  width: 60px
  height: 30px
  background: rgba(255, 255, 255, 0.2)
  border-radius: 20px
  position: relative
  cursor: pointer
  transition: all 0.3s ease
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

    // Icône lune
    &::before
      content: '🌙'
      position: absolute
      left: 50%
      top: 50%
      transform: translate(-50%, -50%)
      font-size: 16px

    &.toggle-light
      transform: translateX(30px)
      background: #333
      &::before
        content: '☀️'

@keyframes glow
  0%, 100%
    box-shadow: 0 0 5px rgba(255, 255, 255, 0.3)
  50%
    box-shadow: 0 0 20px rgba(255, 255, 255, 0.5)

.animate-glow
  animation: glow 2s ease-in-out infinite

.theme-toggle
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
