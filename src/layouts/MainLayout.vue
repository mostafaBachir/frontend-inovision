<script setup>
import NavbarMain from 'src/components/layout/NavbarMain.vue'
import SidebarMain from 'src/components/layout/SidebarMain.vue'
import { ref } from 'vue'
import { useQuasar } from 'quasar'

const showSidebar = ref(false)
const isAuthenticated = ref(true)
const $q = useQuasar()
const isDark = ref($q.dark.isActive)

function handleToggleSidebar() {
  showSidebar.value = !showSidebar.value
}
function handleToggleDark(val) {
  $q.dark.set(val)
  isDark.value = val
}
</script>

<template>
  <q-layout view="hHh lpR fFf">
    <NavbarMain
      :is-authenticated="isAuthenticated"
      :is-dark="isDark"
      @toggle-sidebar="handleToggleSidebar"
      @toggle-dark="handleToggleDark"
      @set-lang="
        (lang) => {
          /* gestion langue */
        }
      "
    />

    <SidebarMain :visible="showSidebar" @close="showSidebar = false" />

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>
