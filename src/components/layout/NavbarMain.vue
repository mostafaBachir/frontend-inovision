<template>
  <q-header elevated class="navbar-main">
    <q-toolbar>
      <q-btn flat round dense icon="menu" @click="$emit('toggle-sidebar')" class="q-mr-sm" />
      <q-avatar size="36px"><img src="~assets/ino_navbar.png" /></q-avatar>
      <span class="text-h6 q-ml-sm text-glow">Inovision</span>
      <q-space />
      <div class="row items-center gt-xs">
        <q-btn flat dense label="Home" to="/" class="navbar-link hover-glow" />
        <q-btn flat dense label="About" to="/about" class="navbar-link hover-glow" />
      </div>
      <q-btn-dropdown flat dense icon="language" class="q-ml-md navbar-link" />
      <!-- Ajoute ici tes toggles si besoin -->
    </q-toolbar>
  </q-header>
</template>

<script setup>
defineProps({
  isAuthenticated: Boolean,
  isDark: Boolean,
})
</script>

<style lang="sass" scoped>
@import 'src/css/quasar.variables.sass'

.navbar-main
  border-radius: 24px !important
  margin: 20px auto 0 auto
  max-width: 1400px
  width: 96vw
  box-shadow: 0 8px 24px #00B8FF22
  overflow: hidden
  background: $sidebar-dark-bg
  color: $sidebar-dark-color
  backdrop-filter: blur(18px)
  border: 1px solid rgba(255,255,255,0.08)
  transition: background 0.3s, color 0.3s

  :root[data-theme="light"] &
    background: $sidebar-light-bg
    color: $sidebar-light-color
    border: 1px solid rgba(0,0,0,0.06)

.brand-text
  color: $sidebar-accent

.navbar-link
  font-family: 'Ubuntu', sans-serif
  text-transform: none
  font-size: 0.98rem
  font-weight: 500
  letter-spacing: 0.01em
  padding: 0 12px
  color: $sidebar-dark-color !important
  border-radius: 14px
  transition: background 0.18s, color 0.18s

  &:hover,
  &:focus
    background: $accent !important
    color: #fff !important
    box-shadow: 0 2px 12px rgba($accent, 0.12)

  :root[data-theme="light"] &
    color: $sidebar-light-color !important

    &:hover,
    &:focus
      background: $accent !important
      color: #fff !important

.navbar-btn
  color: $sidebar-dark-color !important
  :root[data-theme="light"] &
    color: $sidebar-light-color !important

// Responsive: barre pleine largeur et radius 0
@media (max-width: 700px)
  .navbar-main
    max-width: 100vw
    margin: 0
    border-radius: 0
    width: 100vw
    left: 0
</style>
