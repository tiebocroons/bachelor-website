<script setup>
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import heroModelSrc from './assets/model.glb?url'

const navLinks = [
  { label: 'Home', href: '#home' },
  { label: 'Over ons', href: '#over-ons' },
  { label: 'Onderzoek', href: '#onderzoek' },
  { label: 'Contact', href: '#contact' },
]

const highlightCards = [
  {
    title: 'Wat is Sonaris?',
    body: 'Sonaris is een digitale applicatie die zorgprofessionals ondersteunt bij het analyseren en interpreteren van audiogrammen. Door complexe meetgegevens automatisch te structureren en te vertalen naar duidelijke, evidence-based classificaties, helpt Sonaris om sneller tot consistente en transparante inzichten te komen. De tool fungeert daarbij als beslissingsondersteuning: hij versterkt het klinisch oordeel van de professional zonder dit over te nemen.',
  },
  {
    title: 'Waarom Sonaris ertoe doet?',
    body: 'Dit is bijzonder relevant voor de doelgroep van audiologen, KNO-artsen en andere hoorzorgprofessionals, die vandaag geconfronteerd worden met toenemende tijdsdruk, een groeiende hoeveelheid data en de nood aan uniforme interpretatie. Sonaris draagt bij aan efficiëntere workflows, vermindert interpretatievariatie en ondersteunt kwaliteitsvolle, ethisch verantwoorde zorgverlening.',
  },
]

const products = Array.from({ length: 4 }, (_, i) => ({
  id: i,
  title: 'Lorem ipsum',
  body: 'Korte uitleg van het product in één of twee regels.',
}))

const testimonials = [
  {
    id: 1,
    rating: 5,
    quote:
      '“Sterke impact in een dag: duidelijke inzichten, minder ruis en snel toepasbaar in de praktijk.”',
    name: 'Dr. Sophie van den Berg',
    role: 'KNO-arts',
  },
  {
    id: 2,
    rating: 5,
    quote:
      '“De combinatie van interpretatie en rapportage maakt doorverwijzen sneller en consistenter.”',
    name: 'A. de Vries',
    role: 'Audicien',
  },
  {
    id: 3,
    rating: 5,
    quote:
      '“Minder administratieve stappen, meer focus op de patiënt. Precies waar we behoefte aan hebben.”',
    name: 'M. Jansen',
    role: 'Zorgprofessional',
  },
]

const activeTestimonial = ref(1)
const pageRef = ref(null)

let animationContext

function formatStatValue(value, decimals, prefix = '', suffix = '') {
  const formattedValue =
    decimals > 0 ? Number(value).toFixed(decimals) : String(Math.round(Number(value)))

  return `${prefix}${formattedValue}${suffix}`
}

const activeIndex = computed(() => {
  const idx = testimonials.findIndex((t) => t.id === activeTestimonial.value)
  return Math.max(0, idx)
})

onMounted(() => {
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    return
  }

  gsap.registerPlugin(ScrollTrigger)

  animationContext = gsap.context(() => {
    const clearRevealProps = 'transform,opacity,visibility'
    const textReveal = {
      autoAlpha: 0,
      y: 24,
      duration: 0.72,
      ease: 'power3.out',
      clearProps: clearRevealProps,
    }
    const cardReveal = {
      autoAlpha: 0,
      y: 34,
      scale: 0.975,
      duration: 0.88,
      ease: 'power3.out',
      clearProps: clearRevealProps,
    }
    const mediaReveal = {
      autoAlpha: 0,
      x: 24,
      y: 20,
      scale: 0.94,
      duration: 0.96,
      ease: 'power3.out',
      clearProps: clearRevealProps,
    }

    gsap
      .timeline({ defaults: { ease: 'power3.out' } })
      .from('.hero-subtitle, .hero-body, .hero-actions', {
        ...textReveal,
        y: 30,
        duration: 0.84,
        stagger: 0.1,
      })
      .from(
        '.hero-media',
        {
          ...mediaReveal,
          x: 30,
        },
        '-=0.48'
      )

    gsap.utils.toArray('.highlight').forEach((element) => {
      gsap.from(element, {
        ...cardReveal,
        scrollTrigger: {
          trigger: element,
          start: 'top 82%',
          once: true,
        },
      })
    })

    gsap.from('.stats', {
      ...cardReveal,
      scrollTrigger: {
        trigger: '.stats',
        start: 'top 82%',
        once: true,
      },
    })

    gsap.from('.bap-item', {
      ...textReveal,
      x: 26,
      y: 0,
      duration: 0.78,
      stagger: 0.12,
      scrollTrigger: {
        trigger: '.bap-list',
        start: 'top 84%',
        once: true,
      },
    })

    gsap.from('.usp-card', {
      ...cardReveal,
      scrollTrigger: {
        trigger: '.usp-card',
        start: 'top 82%',
        once: true,
      },
    })

    gsap
      .timeline({
        scrollTrigger: {
          trigger: '.products',
          start: 'top 82%',
          once: true,
        },
      })
      .from('.products .section-title', textReveal)
      .from(
        '.product-card',
        {
          ...cardReveal,
          duration: 0.8,
          stagger: 0.08,
        },
        '-=0.28'
      )

    gsap
      .timeline({
        scrollTrigger: {
          trigger: '.verdict',
          start: 'top 82%',
          once: true,
        },
      })
      .from('.verdict .section-title', textReveal)
      .from(
        '.testimonial',
        {
          ...cardReveal,
          y: 28,
          scale: 0.985,
          duration: 0.78,
          stagger: 0.08,
        },
        '-=0.26'
      )

    gsap
      .timeline({
        scrollTrigger: {
          trigger: '.newsletter',
          start: 'top 84%',
          once: true,
        },
      })
      .from('.newsletter .section-title, .newsletter-body', {
        ...textReveal,
        stagger: 0.1,
      })
      .from(
        '.newsletter-form',
        {
          ...cardReveal,
          y: 26,
          scale: 0.985,
          duration: 0.78,
        },
        '-=0.22'
      )

    gsap.from('.footer-left, .footer-col', {
      ...textReveal,
      y: 18,
      stagger: 0.08,
      scrollTrigger: {
        trigger: '.footer',
        start: 'top bottom',
        once: true,
      },
    })

    gsap.utils.toArray('.stat-value[data-count-to]').forEach((element) => {
      const targetValue = Number(element.dataset.countTo ?? '0')
      const decimals = Number(element.dataset.decimals ?? '0')
      const prefix = element.dataset.prefix ?? ''
      const suffix = element.dataset.suffix ?? ''
      const counter = { value: 0 }

      element.textContent = formatStatValue(0, decimals, prefix, suffix)

      gsap.from(element, {
        ...textReveal,
        y: 18,
        scrollTrigger: {
          trigger: element,
          start: 'top 86%',
          once: true,
        },
      })

      gsap.to(counter, {
        value: targetValue,
        duration: 1.4,
        ease: 'power2.out',
        onUpdate: () => {
          element.textContent = formatStatValue(counter.value, decimals, prefix, suffix)
        },
        scrollTrigger: {
          trigger: element,
          start: 'top 86%',
          once: true,
        },
      })
    })
  }, pageRef.value)
})

onBeforeUnmount(() => {
  animationContext?.revert()
})

function prevTestimonial() {
  const next = (activeIndex.value - 1 + testimonials.length) % testimonials.length
  activeTestimonial.value = testimonials[next].id
}

function nextTestimonial() {
  const next = (activeIndex.value + 1) % testimonials.length
  activeTestimonial.value = testimonials[next].id
}

function setTestimonial(id) {
  activeTestimonial.value = id
}

function scrollToAnchor(href) {
  const id = href?.startsWith('#') ? href.slice(1) : href
  const el = document.getElementById(id)
  el?.scrollIntoView({ behavior: 'smooth', block: 'start' })
}
</script>

<template>
  <div ref="pageRef" class="page" id="home">
    <header class="topbar">
      <a class="nav-logo" href="#home" @click.prevent="scrollToAnchor('#home')">Sonaris</a>
      <nav class="nav">
        <a
          v-for="link in navLinks"
          :key="link.href"
          class="nav-link"
          :href="link.href"
          @click.prevent="scrollToAnchor(link.href)"
        >
          {{ link.label }}
        </a>
      </nav>

      <a class="nav-cta" href="#contact" @click.prevent="scrollToAnchor('#contact')">
        Get more info
      </a>
      
    </header>
    <section class="hero" aria-label="Hero">
        <div class="hero-inner">
          <div class="hero-copy">
            <h1 class="hero-subtitle">
              Slimmere gehoortest-interpretatie voor de juiste keuze tussen hoorapparaat of gehoorimplantaat
            </h1>
            <p class="hero-body">
              Sonaris analyseert audiogramresultaten en vertaalt ze naar heldere aanbevelingen — zodat zorgprofessionals sneller en zekerder kunnen beslissen welke oplossing het beste past bij de patiënt.
            </p>

            <div class="hero-actions">
              <a class="btn btn-primary" href="#over-ons" @click.prevent="scrollToAnchor('#over-ons')">
                Ontdek het concept
              </a>
            </div>
          </div>

          <div class="hero-media" aria-label="3D model">
            <model-viewer
              class="hero-model"
              :src="heroModelSrc"
              alt="3D model"
              autoplay
              auto-rotate
              auto-rotate-delay="0"
              rotation-per-second="40deg"
              camera-orbit="30deg 70deg auto"
              camera-controls
              disable-zoom
              interaction-prompt="auto"
              shadow-intensity="0.6"
              environment-image="neutral"
            />
          </div>
        </div>

      </section>
    <main class="main">

      <section class="highlights" id="over-ons" aria-label="Highlights">
        <div class="highlight" v-for="card in highlightCards" :key="card.title">
          <div class="highlight-copy">
            <h3 class="section-title">{{ card.title }}</h3>
            <p class="section-body">{{ card.body }}</p>
          </div>
          <div class="highlight-media" aria-hidden="true">
            <img src="/Portrait%20of%20a%20lady%20with%20a%20cochlear%20implant..jpg" alt="Highlight Image" class="img-placeholder" />
          </div>
        </div>
      </section>

      <section class="bap" id="onderzoek" aria-label="Onderzoek">
        <div class="bap-bottom">
          <div class="stats">
            <div class="stat-kicker">BAMABAMBAMBA</div>
            <div class="stats-grid">
              <div class="stat">
                <div class="stat-value" data-count-to="1.5" data-decimals="1" data-suffix="+mld">
                  1.5+mld
                </div>
                <div class="stat-label">Introduction copy lorem ipsum dolor.</div>
              </div>
              <div class="stat">
                <div class="stat-value" data-count-to="90" data-decimals="0" data-suffix="%+">
                  90%+
                </div>
                <div class="stat-label">Of people who medically qualify for a Cochlear Implant (CI) are currently under-treated</div>
              </div>
            </div>
            <img src="/_24A0522.jpg" alt="Stat Image" class="stat-card" aria-hidden="true" />
          </div>

          <div class="bap-list">
            <article class="bap-item">
              <img src="/kopf+AP.jpg" alt="Bap Item Image" class="bap-item-image" aria-hidden="true" />
              <div>
                <h4 class="bap-item-title">Lorem ipsum dolor</h4>
                <p class="bap-item-body">
                  Snelle interpretatie met vaste terminologie — makkelijker vergelijken en doorverwijzen.
                </p>
              </div>
            </article>
            <article class="bap-item">
              <img src="/Portrait%20of%20a%20lady%20with%20a%20cochlear%20implant..jpg" alt="Bap Item Image" class="bap-item-image" aria-hidden="true" />
              <div>
                <h4 class="bap-item-title">Lorem ipsum dolor</h4>
                <p class="bap-item-body">
                  Automatische samenvatting in een consistent format voor dossier, verwijzing en overleg.
                </p>
              </div>
            </article>
            <article class="bap-item">
              <img src="/_24A0522.jpg" alt="Bap Item Image" class="bap-item-image" aria-hidden="true" />
              <div>
                <h4 class="bap-item-title">Lorem ipsum dolor</h4>
                <p class="bap-item-body">
                  Duidelijke output met focus op relevantie, zodat je sneller tot een besluit komt.
                </p>
              </div>
            </article>
          </div>
        </div>
      </section>

      <section class="usp" aria-label="USP">
        <div class="usp-card">
          <div>
            <h3 class="usp-title">USP 1</h3>
            <p class="usp-body">
              Eén heldere boodschap die de grootste waardepropositie samenvat.
            </p>
            <a class="btn btn-dark" href="#contact" @click.prevent="scrollToAnchor('#contact')">Lees meer</a>
          </div>
          <img src="/kopf+AP.jpg" alt="USP Image" class="usp-media" aria-hidden="true" />
        </div>
      </section>

      <section class="products" aria-label="Products">
        <h3 class="section-title centered">Our products</h3>
        <div class="product-grid">
          <article v-for="p in products" :key="p.id" class="product-card">
            <img src="/Portrait%20of%20a%20lady%20with%20a%20cochlear%20implant..jpg" alt="Product Image" class="product-image" aria-hidden="true" />
            <h4 class="product-title">{{ p.title }}</h4>
            <p class="product-body">{{ p.body }}</p>
            <a class="btn btn-dark" href="#contact" @click.prevent="scrollToAnchor('#contact')">Lees meer</a>
          </article>
        </div>
      </section>

      <section class="verdict" aria-label="Testimonials">
        <h3 class="section-title centered">What’s the verdict</h3>

        <div class="carousel">
          <button class="carousel-arrow" type="button" @click="prevTestimonial" aria-label="Vorige">
            ‹
          </button>

          <div class="carousel-track" role="group" aria-label="Testimonials">
            <article
              v-for="t in testimonials"
              :key="t.id"
              class="testimonial"
              :class="{ active: t.id === activeTestimonial }"
              @click="setTestimonial(t.id)"
              tabindex="0"
            >
              <div class="stars" aria-hidden="true">
                <span v-for="n in t.rating" :key="n" class="star">★</span>
              </div>
              <p class="quote">{{ t.quote }}</p>
              <div class="person">
                <img src="/_24A0522.jpg" alt="Testimonial Avatar" class="avatar" aria-hidden="true" />
                <div>
                  <div class="person-name">{{ t.name }}</div>
                  <div class="person-role">{{ t.role }}</div>
                </div>
              </div>
            </article>
          </div>

          <button class="carousel-arrow" type="button" @click="nextTestimonial" aria-label="Volgende">
            ›
          </button>
        </div>
      </section>

      <section class="newsletter" aria-label="Newsletter">
        <h3 class="section-title centered">Hear our latest</h3>
        <p class="newsletter-body">Schrijf je in voor updates over het concept en de voortgang.</p>
        <form class="newsletter-form" @submit.prevent>
          <input class="input" type="email" autocomplete="email" placeholder="Your email address" />
          <button class="btn btn-primary" type="submit">Sign up</button>
        </form>
      </section>
    </main>

    <footer class="footer" id="contact" aria-label="Footer">
      <div class="footer-inner">
        <div class="footer-left">
          <div class="footer-brand">Sonaris</div>
          <div class="footer-social" aria-label="Social links">
            <a class="social-dot" href="#" aria-label="Facebook">
              <svg xmlns="http://www.w3.org/2000/svg" width="10" height="10" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M18 2h-3a5 5 0 0 0-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 0 1 1-1h3z"/></svg>
            </a>
            <a class="social-dot" href="#" aria-label="YouTube">
              <svg xmlns="http://www.w3.org/2000/svg" width="11" height="11" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M22.54 6.42a2.78 2.78 0 0 0-1.95-1.96C18.88 4 12 4 12 4s-6.88 0-8.59.46A2.78 2.78 0 0 0 1.46 6.42 29 29 0 0 0 1 12a29 29 0 0 0 .46 5.58 2.78 2.78 0 0 0 1.95 1.96C5.12 20 12 20 12 20s6.88 0 8.59-.46a2.78 2.78 0 0 0 1.95-1.96A29 29 0 0 0 23 12a29 29 0 0 0-.46-5.58z"/><polygon points="9.75 15.02 15.5 12 9.75 8.98 9.75 15.02" fill="var(--color-accent)"/></svg>
            </a>
            <a class="social-dot" href="#" aria-label="X (Twitter)">
              <svg xmlns="http://www.w3.org/2000/svg" width="10" height="10" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
            </a>
            <a class="social-dot" href="#" aria-label="Instagram">
              <svg xmlns="http://www.w3.org/2000/svg" width="10" height="10" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><rect x="2" y="2" width="20" height="20" rx="5" ry="5"/><path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"/><line x1="17.5" y1="6.5" x2="17.51" y2="6.5"/></svg>
            </a>
          </div>
        </div>
        <div class="footer-cols">
          <div class="footer-col">
            <div class="footer-heading">Product</div>
            <a class="footer-link" href="#onderzoek" @click.prevent="scrollToAnchor('#onderzoek')">Onderzoek</a>
            <a class="footer-link" href="#over-ons" @click.prevent="scrollToAnchor('#over-ons')">Over ons</a>
          </div>
          <div class="footer-col">
            <div class="footer-heading">Resources</div>
            <a class="footer-link" href="#">Privacy</a>
            <a class="footer-link" href="#">Voorwaarden</a>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<style scoped>
.page {
  color: var(--color-text);
  overflow-x: hidden;
}

.topbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 10;
  background: #ffffff;
  border-bottom: 1px solid color-mix(in srgb, var(--color-text) 8%, transparent);
  box-shadow: 0 8px 24px rgba(22, 26, 29, 0.06);
  padding: 14px 18px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.nav {
  display: flex;
  gap: 18px;
  flex-wrap: wrap;
}

.nav-link {
  color: var(--color-text);
  text-decoration: none;
  font-size: 13px;
  transition:
    color 0.28s ease,
    transform 0.28s cubic-bezier(0.22, 1, 0.36, 1);
}

.nav-link:hover {
  text-decoration: underline;
}

.nav-logo {
  font-size: 18px;
  font-weight: 800;
  letter-spacing: -0.02em;
  color: var(--color-text);
  text-decoration: none;
}

.nav-cta {
  text-decoration: none;
  font-size: 12px;
  padding: 8px 12px;
  border-radius: 999px;
  background: #000000;
  color: #ffffff;
  transition:
    transform 0.3s cubic-bezier(0.22, 1, 0.36, 1),
    box-shadow 0.3s ease,
    background-color 0.3s ease;
}

.hero {
  position: relative;
  overflow: hidden;
  padding: 120px 18px 80px;
  background-image: url('./assets/wave-red.svg');
  background-repeat: no-repeat;
  background-position: 75% 110%;
  background-size: auto;
}

@media (min-width: 1200px) {
  .hero {
    background-size: 110vw auto;
    background-position: 80% 120%;
  }
}

.hero-inner {
  max-width: 1080px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1.2fr 0.8fr;
  gap: 28px;
  align-items: center;
}

.hero-subtitle {
  font-size: clamp(1.8rem, 3vw, 2.8rem);
  font-weight: 800;
  line-height: 1.15;
  letter-spacing: -0.02em;
  max-width: 18ch;
}

.hero-body {
  margin-top: 14px;
  max-width: 44ch;
  font-size: 15px;
  line-height: 1.7;
  color: color-mix(in srgb, var(--color-text) 85%, transparent);
}

.hero-actions {
  margin-top: 14px;
}

.hero-media {
  display: grid;
  place-items: center;
}

.hero-model {
  width: 460px;
  height: 460px;
  max-width: 100%;
  background: transparent;
}


.tagline {
  padding: 60px 18px 34px;
  text-align: center;
}

.tagline-title {
  font-weight: 700;
  font-size: 14px;
}

.tagline-sub {
  margin-top: 6px;
  color: color-mix(in srgb, var(--color-text) 70%, transparent);
  font-size: 12px;
}

.tagline-icons {
  margin-top: 14px;
  display: flex;
  justify-content: center;
  gap: 18px;
}

.mini-icon {
  width: 28px;
  height: 28px;
  border: 1px solid var(--color-border);
  border-radius: 6px;
  background: var(--color-background-soft);
}

.highlights {
  max-width: 1080px;
  margin: 0 auto;
  padding: 18px;
  display: grid;
  gap: 26px;
}

.highlight {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 18px;
  align-items: center;
  transition:
    transform 0.4s cubic-bezier(0.22, 1, 0.36, 1),
    box-shadow 0.4s ease;
}

.highlight:nth-child(even) {
  direction: rtl;
}

.highlight:nth-child(even) > * {
  direction: ltr;
}

.section-title {
  font-size: 16px;
  font-weight: 700;
}

.section-title.centered {
  text-align: center;
}

.section-body {
  margin-top: 8px;
  max-width: 60ch;
  color: color-mix(in srgb, var(--color-text) 70%, transparent);
}

.img-placeholder {
  width: 100%;
  aspect-ratio: 16 / 10;
  object-fit: cover;
  display: block;
  border-radius: 10px;
  box-shadow: 0 20px 40px rgba(22, 26, 29, 0.08);
}

.micro {
  max-width: 1080px;
  margin: 0 auto;
  padding: 18px;
}

.micro-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 14px;
}

.micro-card {
  text-align: center;
  padding: 16px 12px;
}

.micro-icon {
  width: 34px;
  height: 34px;
  margin: 0 auto 10px;
  border-radius: 8px;
  border: 1px solid var(--color-border);
  background: var(--color-background-soft);
}

.micro-title {
  font-size: 12px;
  font-weight: 700;
}

.micro-body {
  margin-top: 6px;
  font-size: 11px;
  color: color-mix(in srgb, var(--color-text) 70%, transparent);
}

.bap {
  position: relative;
  overflow: hidden;
  padding: 40px 18px 26px;
  background-size: 900px auto;
}
.main {
  position: relative;
}

.main::before {
  content: '';
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 130vw;
  height: 100%;
  background-image: url('./assets/wave-gray.svg');
  background-repeat: no-repeat;
  background-position: center top;
  background-size: 120% auto;
  pointer-events: none;
  z-index: -99;
}

.bap-bottom {
  max-width: 1080px;
  margin: 22px auto 0;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 100px;
  align-items: center;
}

.stats {
  display: grid;
  gap: 12px;
}

.stats-grid {
  display: flex;
  gap: 14px;
}

.stat {
  flex: 1;
  padding: 10px 0;
}

.stat-kicker {
  font-size: 10px;
  letter-spacing: 0.12em;
  opacity: 0.7;
}

.stat-value {
  font-size: 32px;
  font-weight: 800;
  color: var(--color-accent);
}

.stat-label {
  margin-top: 4px;
  font-size: 11px;
  color: color-mix(in srgb, var(--color-text) 70%, transparent);
}

.stat-card {
  width: 100%;
  aspect-ratio: 4 / 3;
  object-fit: cover;
  border-radius: 10px;
  display: block;
}

.bap-list {
  padding-top: 25%;
  display: grid;
  gap: 28px;
  align-content: start;
}

.bap-item {
  display: flex;
  gap: 14px;
  align-items: flex-start;
}

.bap-item-image {
  width: 80px;
  min-width: 80px;
  flex-shrink: 0;
  aspect-ratio: 4 / 3;
  object-fit: cover;
  display: block;
  border-radius: 6px;
}

.bap-item-title {
  font-size: 13px;
  font-weight: 700;
}

.bap-item-body {
  margin-top: 4px;
  font-size: 11px;
  color: color-mix(in srgb, var(--color-text) 70%, transparent);
}

.usp {
  padding: 18px;
  z-index: 99;
}

.usp-card {
  max-width: 1080px;
  margin: 0 auto;
  background: #c0c0c0;
  color: #ffffff;
  border-radius: 14px;
  padding: 18px;
  display: grid;
  grid-template-columns: 1.2fr 0.8fr;
  gap: 18px;
  align-items: center;
  z-index: 99;
  box-shadow: 0 24px 48px rgba(22, 26, 29, 0.08);
  transition:
    transform 0.4s cubic-bezier(0.22, 1, 0.36, 1),
    box-shadow 0.4s ease;
}

.usp-title {
  font-weight: 800;
  font-size: 16px;
  z-index: 99;
}

.usp-body {
  margin-top: 6px;
  color: color-mix(in srgb, var(--color-text) 70%, transparent);
}

.usp-media {
  width: 100%;
  aspect-ratio: 16 / 9;
  object-fit: cover;
  border-radius: 10px;
  display: block;
}

.products {
  max-width: 1080px;
  margin: 0 auto;
  padding: 24px 18px 14px;
}

.product-grid {
  margin-top: 14px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 14px;
}

.product-card {
  padding: 10px;
  transition:
    transform 0.38s cubic-bezier(0.22, 1, 0.36, 1),
    box-shadow 0.38s ease;
}

.product-image {
  width: 100%;
  aspect-ratio: 4 / 3;
  object-fit: cover;
  border-radius: 10px;
  display: block;
}

.product-title {
  margin-top: 10px;
  font-weight: 800;
  font-size: 12px;
}

.product-body {
  margin-top: 6px;
  font-size: 11px;
  color: color-mix(in srgb, var(--color-text) 70%, transparent);
}

.verdict {
  max-width: 1080px;
  margin: 0 auto;
  padding: 16px 18px 28px;
}

.carousel {
  margin-top: 12px;
  display: grid;
  grid-template-columns: 42px 1fr 42px;
  gap: 10px;
  align-items: center;
}

.carousel-arrow {
  width: 42px;
  height: 42px;
  border-radius: 999px;
  border: 1px solid var(--color-border);
  background: var(--color-background);
  cursor: pointer;
  font-size: 22px;
  line-height: 0;
  transition:
    transform 0.28s cubic-bezier(0.22, 1, 0.36, 1),
    box-shadow 0.28s ease,
    border-color 0.28s ease;
}

.carousel-track {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 12px;
}

.testimonial {
  border: 1px solid var(--color-border);
  border-radius: 12px;
  padding: 14px;
  background: #ffffff;
  cursor: pointer;
  opacity: 0.75;
  box-shadow: 0 18px 36px rgba(22, 26, 29, 0.05);
  transition:
    transform 0.34s cubic-bezier(0.22, 1, 0.36, 1),
    opacity 0.34s ease,
    box-shadow 0.34s ease,
    border-color 0.34s ease;
}

.testimonial.active {
  opacity: 1;
  transform: translateY(-4px);
  border-color: color-mix(in srgb, var(--color-accent) 24%, var(--color-border));
  box-shadow: 0 24px 44px rgba(230, 27, 46, 0.12);
}

.stars {
  color: var(--color-accent);
  font-size: 12px;
}

.quote {
  margin-top: 10px;
  font-size: 11px;
  color: color-mix(in srgb, var(--color-text) 75%, transparent);
}

.person {
  margin-top: 12px;
  display: grid;
  grid-template-columns: 26px 1fr;
  gap: 10px;
  align-items: center;
}

.avatar {
  width: 26px;
  height: 26px;
  border-radius: 999px;
  object-fit: cover;
  display: block;
  flex-shrink: 0;
}

.person-name {
  font-size: 11px;
  font-weight: 700;
}

.person-role {
  font-size: 10px;
  opacity: 0.7;
}

.newsletter {
  padding: 22px 18px 30px;
  text-align: center;
}

.newsletter-body {
  margin-top: 8px;
  color: color-mix(in srgb, var(--color-text) 70%, transparent);
  font-size: 12px;
}

.newsletter-form {
  margin-top: 12px;
  display: grid;
  grid-template-columns: 1fr 160px;
  gap: 10px;
  max-width: 520px;
  margin-left: auto;
  margin-right: auto;
}

.input {
  height: 42px;
  padding: 10px 12px;
  border-radius: 8px;
  border: 1px solid var(--color-border);
  background: #ffffff;
  outline: none;
}

.input:focus {
  border-color: color-mix(in srgb, var(--color-accent) 60%, var(--color-border));
}

.footer {
  background: var(--color-accent);
  color: var(--vt-c-white);
  margin-top: 10px;
}

.footer-inner {
  max-width: 1080px;
  margin: 0 auto;
  padding: 18px;
  display: flex;
  justify-content: space-between;
  gap: 18px;
  flex-wrap: wrap;
}

.footer-brand {
  font-weight: 800;
}

.footer-social {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}

.social-dot {
  width: 28px;
  height: 28px;
  border-radius: 999px;
  border: 1px solid color-mix(in srgb, var(--vt-c-white) 60%, transparent);
  background: color-mix(in srgb, var(--vt-c-white) 20%, transparent);
  display: inline-flex;
  align-items: center;
  justify-content: center;
  color: var(--vt-c-white);
  transition:
    background 0.28s ease,
    transform 0.28s cubic-bezier(0.22, 1, 0.36, 1);
}

.social-dot:hover {
  background: color-mix(in srgb, var(--vt-c-white) 35%, transparent);
}

.footer-cols {
  display: flex;
  gap: 28px;
}

.footer-heading {
  font-weight: 800;
  margin-bottom: 8px;
}

.footer-link {
  display: block;
  color: var(--vt-c-white);
  text-decoration: none;
  opacity: 0.9;
  font-size: 12px;
  margin-top: 6px;
}

.footer-link:hover {
  opacity: 1;
  text-decoration: underline;
}

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  height: 40px;
  padding: 0 14px;
  border-radius: 999px;
  border: 1px solid transparent;
  text-decoration: none;
  font-size: 12px;
  font-weight: 700;
  cursor: pointer;
  user-select: none;
  transition:
    transform 0.28s cubic-bezier(0.22, 1, 0.36, 1),
    filter 0.28s ease,
    box-shadow 0.28s ease;
}

.btn-primary {
  background: var(--color-accent);
  color: var(--vt-c-white);
}

.btn-primary:hover {
  filter: brightness(0.95);
}

.btn-dark {
  background: var(--vt-c-black);
  color: var(--vt-c-white);
}

.btn-dark:hover {
  filter: brightness(1.05);
}

@media (hover: hover) and (pointer: fine) {
  .nav-link:hover {
    color: var(--color-accent);
    transform: translateY(-1px);
    text-decoration: none;
  }

  .nav-cta:hover,
  .btn:hover,
  .carousel-arrow:hover,
  .social-dot:hover {
    transform: translateY(-2px);
  }

  .nav-cta:hover,
  .btn:hover,
  .carousel-arrow:hover {
    box-shadow: 0 14px 28px rgba(22, 26, 29, 0.12);
  }

  .highlight:hover,
  .usp-card:hover,
  .product-card:hover,
  .testimonial:hover {
    transform: translateY(-6px);
    box-shadow: 0 26px 48px rgba(22, 26, 29, 0.1);
  }

  .testimonial:hover {
    opacity: 1;
    border-color: color-mix(in srgb, var(--color-accent) 18%, var(--color-border));
  }

  .social-dot:hover {
    background: color-mix(in srgb, var(--vt-c-white) 35%, transparent);
  }
}

/* ── Tablet (≤ 900px) ── */
@media (max-width: 900px) {
  .hero-inner {
    grid-template-columns: 1fr;
  }

  .hero-model {
    width: 260px;
    height: 260px;
  }

  .highlight {
    grid-template-columns: 1fr;
  }

  .bap-bottom {
    grid-template-columns: 1fr;
  }

  .usp-card {
    grid-template-columns: 1fr;
  }

  .product-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .carousel {
    grid-template-columns: 1fr;
  }

  .carousel-arrow {
    display: none;
  }

  .carousel-track {
    grid-template-columns: 1fr;
  }

  .newsletter-form {
    grid-template-columns: 1fr;
  }
}

/* ── Phone (≤ 600px) ── */
@media (max-width: 600px) {
  .topbar {
    padding: 10px 14px;
    flex-wrap: wrap;
    gap: 8px;
  }

  .nav {
    order: 3;
    width: 100%;
    justify-content: center;
    gap: 14px;
  }

  .hero {
    padding: 100px 14px 24px;
  }

  .hero-subtitle {
    font-size: 22px;
    line-height: 1.25;
  }

  .hero-body {
    font-size: 13px;
  }

  .hero-model {
    width: 200px;
    height: 200px;
  }

  .highlights {
    padding: 14px;
  }

  .stats-grid {
    flex-direction: column;
  }

  .bap-item-image {
    width: 60px;
    min-width: 60px;
  }

  .product-grid {
    grid-template-columns: 1fr;
  }

  .carousel-track {
    grid-template-columns: 1fr;
  }

  .usp-card {
    grid-template-columns: 1fr;
  }

  .newsletter-form {
    grid-template-columns: 1fr;
  }

  .footer-inner {
    flex-direction: column;
  }

  .footer-cols {
    flex-direction: column;
    gap: 16px;
  }
}
</style>
