<template>
  <transition name="fade-only">
    <aside v-show="visible" class="sidebar-main">
      <!-- Fermeture mobile -->
      <q-btn
        v-if="$q.screen.lt.md"
        flat
        round
        icon="close"
        class="close-sidebar-btn"
        @click="$emit('close')"
        style="position: absolute; top: 16px; right: 16px; z-index: 10"
      />
      <div class="sidebar-scroll">
        <q-list padding>
          <q-item-label header class="sidebar-title">Inovision</q-item-label>
          <q-item clickable to="/dashboard" active-class="active-link" @click="$emit('close')">
            <q-item-section avatar><q-icon name="dashboard" /></q-item-section>
            <q-item-section>Dashboard</q-item-section>
          </q-item>
          <q-item clickable to="/ocr" active-class="active-link" @click="$emit('close')">
            <q-item-section avatar><q-icon name="receipt_long" /></q-item-section>
            <q-item-section>Receipts</q-item-section>
          </q-item>
          <q-item clickable to="/profile" active-class="active-link" @click="$emit('close')">
            <q-item-section avatar><q-icon name="person" /></q-item-section>
            <q-item-section>Profile</q-item-section>
          </q-item>
          <q-item clickable to="/settings" active-class="active-link" @click="$emit('close')">
            <q-item-section avatar><q-icon name="settings" /></q-item-section>
            <q-item-section>Settings</q-item-section>
          </q-item>
          <q-separator class="q-my-sm" />
          <q-item clickable @click="logout">
            <q-item-section avatar>
              <q-icon name="logout" color="negative" />
            </q-item-section>
            <q-item-section class="text-negative">Logout</q-item-section>
          </q-item>
        </q-list>
      </div>
    </aside>
  </transition>
</template>

<script setup>
import { use_auth_store } from 'src/stores/auth-store'
import { useRouter } from 'vue-router' // <-- ICI, OK !

defineProps({ visible: Boolean })
const auth = use_auth_store()
const router = useRouter()
function logout() {
  auth.logout(router)
}
</script>

<style lang="sass" scoped>
@import 'src/css/quasar.variables.sass'

.sidebar-main
  position: fixed
  top: 96px
  left: 50px
  width: 260px
  height: calc(100vh - 132px)
  background: $sidebar-dark-bg
  color: $sidebar-dark-color
  backdrop-filter: blur(16px)
  border-radius: 24px
  box-shadow: 0 12px 24px rgba(0, 184, 255, 0.08)
  border: 1px solid rgba(255, 255, 255, 0.05)
  z-index: 998
  padding: 8px
  display: flex
  flex-direction: column
  transition: background 0.3s, color 0.3s

  // Light mode
  :root[data-theme="light"] &
    background: $sidebar-light-bg
    color: $sidebar-light-color

.sidebar-title
  color: $accent
  font-size: 1.15rem
  letter-spacing: 0.03em

.sidebar-scroll
  flex: 1
  overflow-y: auto
  scrollbar-width: thin
  &::-webkit-scrollbar
    width: 6px
  &::-webkit-scrollbar-thumb
    background-color: rgba(255, 255, 255, 0.18)
    border-radius: 4px

.q-item.active-link
  background: $sidebar-accent
  color: #fff !important
  border-left: 4px solid $sidebar-accent

  :root[data-theme="light"] &
    background: $sidebar-accent
    color: #fff !important
    border-left: 4px solid $sidebar-accent

.q-item:hover
  background-color: rgba($sidebar-accent, 0.10)
  :root[data-theme="light"] &
    background-color: rgba($sidebar-accent, 0.18)

.q-item-section
  color: inherit !important

.q-item-section.text-negative
  color: #FF5252 !important

// Responsive
@media (max-width: 700px)
  .sidebar-main
    left: 0
    width: 100vw
    height: 100vh
    border-radius: 0
    top: 0
    z-index: 999
    box-shadow: 0 0 0 9999px rgba(0,0,0,0.3)

  .close-sidebar-btn
    display: block

@media (min-width: 701px)
  .close-sidebar-btn
    display: none
</style>
