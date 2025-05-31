<template>
  <div class="hero-section">
    <!-- SVG Background Full Screen -->
    <img alt="background" src="~assets/cours.svg" />
    <!-- Contenu Hero -->
    <div class="hero-content absolute-center text-center">
      <q-img src="~assets/ino_traansparent.png" width="200px" class="q-mb-lg" no-spinner />

      <h1 class="text-h2 text-weight-bold q-mb-md">
        {{ $t('hero.title', 'Welcome to INO') }}
      </h1>

      <p class="text-h5 text-weight-light q-mb-xl">
        {{ $t('hero.subtitle', 'Next Generation Platform') }}
      </p>

      <div class="q-gutter-md">
        <q-btn
          size="lg"
          color="primary"
          label="Get Started"
          @click="$router.push('/dashboard')"
          no-caps
          padding="12px 32px"
        />
        <q-btn
          size="lg"
          color="white"
          text-color="primary"
          outline
          label="Learn More"
          no-caps
          padding="12px 32px"
          @click="scrollToNext"
        />
      </div>
    </div>

    <!-- Indicateur de scroll -->
    <div class="scroll-indicator absolute-bottom text-center q-pb-md">
      <q-icon name="expand_more" size="32px" class="bounce-animation" />
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted, computed } from 'vue'
import { useQuasar } from 'quasar'

export default {
  name: 'HeroSection',
  setup() {
    const $q = useQuasar()
    const particlesGroup = ref(null)
    let animationId = null
    let particles = []

    // Positions des lignes animées
    const linePositions = computed(() => [
      { x1: 100, y1: 100, x2: 400, y2: 400 },
      { x1: 1820, y1: 100, x2: 1520, y2: 400 },
      { x1: 960, y1: 980, x2: 960, y2: 680 },
    ])

    // Classe Particle
    class Particle {
      constructor(x, y) {
        this.x = x
        this.y = y
        this.vx = (Math.random() - 0.5) * 2
        this.vy = (Math.random() - 0.5) * 2
        this.life = 1.0
        this.decay = 0.01
        this.element = null
      }

      update() {
        this.x += this.vx
        this.y += this.vy
        this.life -= this.decay

        if (this.element) {
          this.element.setAttribute('cx', this.x)
          this.element.setAttribute('cy', this.y)
          this.element.setAttribute('opacity', this.life)
        }

        return this.life > 0
      }
    }

    // Créer une particule SVG
    const createParticleElement = (particle) => {
      const el = document.createElementNS('http://www.w3.org/2000/svg', 'circle')
      el.setAttribute('r', '3')
      el.setAttribute('fill', $q.dark.isActive ? '#42A5F5' : '#1976D2')
      el.setAttribute('filter', 'url(#glow)')
      particle.element = el
      return el
    }

    // Gestion des particules
    const handleMouseMove = (e) => {
      if (particles.length < 50) {
        const rect = e.currentTarget.getBoundingClientRect()
        const x = (e.clientX - rect.left) * (1920 / rect.width)
        const y = (e.clientY - rect.top) * (1080 / rect.height)

        const particle = new Particle(x, y)
        particles.push(particle)

        if (particlesGroup.value) {
          particlesGroup.value.appendChild(createParticleElement(particle))
        }
      }
    }

    // Animation loop
    const animate = () => {
      particles = particles.filter((particle) => {
        const alive = particle.update()
        if (!alive && particle.element) {
          particle.element.remove()
        }
        return alive
      })

      animationId = requestAnimationFrame(animate)
    }

    // Scroll vers la section suivante
    const scrollToNext = () => {
      window.scrollTo({
        top: window.innerHeight,
        behavior: 'smooth',
      })
    }

    onMounted(() => {
      animate()
      document.addEventListener('mousemove', handleMouseMove)
    })

    onUnmounted(() => {
      if (animationId) {
        cancelAnimationFrame(animationId)
      }
      document.removeEventListener('mousemove', handleMouseMove)
    })

    return {
      particlesGroup,
      linePositions,
      scrollToNext,
    }
  },
}
</script>

<style lang="sass" scoped>
.hero-section
  position: relative
  width: 100vw
  height: 100vh
  overflow: hidden

.hero-svg
  z-index: 0

.hero-content
  z-index: 10
  pointer-events: none

  .q-btn
    pointer-events: all

.scroll-indicator
  z-index: 10
  animation: bounce 2s infinite

@keyframes bounce
  0%, 20%, 50%, 80%, 100%
    transform: translateY(0)
  40%
    transform: translateY(-10px)
  60%
    transform: translateY(-5px)

.bounce-animation
  animation: bounce 2s infinite

// Dark mode adjustments
.body--dark
  .hero-section
    color: white
</style>
