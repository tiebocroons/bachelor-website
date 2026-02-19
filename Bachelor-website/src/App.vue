<script setup>
import { computed, ref } from 'vue'
import heroModelSrc from './assets/model.glb?url'

const navLinks = [
  { label: 'Home', href: '#home' },
  { label: 'Over ons', href: '#over-ons' },
  { label: 'Onderzoek', href: '#onderzoek' },
  { label: 'Contact', href: '#contact' },
]

const featureIcons = Array.from({ length: 5 }, (_, i) => ({ id: i }))

const highlightCards = [
  {
    title: 'Boom',
    body: 'Korte toelichting op het concept en waarom dit relevant is voor de doelgroep.',
  },
  {
    title: 'Bam',
    body: 'Nog een korte toelichting met focus op workflow, inzicht en tijdswinst.',
  },
]

const microCards = Array.from({ length: 4 }, (_, i) => ({
  id: i,
  title: 'Lorem ipsum',
  body: 'Introductie op kernpunt in één zin.',
}))

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
const activeIndex = computed(() => {
  const idx = testimonials.findIndex((t) => t.id === activeTestimonial.value)
  return Math.max(0, idx)
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
  <div class="page" id="home">
    <header class="topbar">
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
        Chat service
      </a>
      
    </header>
    <section class="hero" aria-label="Hero">
        <div class="hero-inner">
          <div class="hero-copy">
            <h1 class="hero-title">Sonaris</h1>
            <h2 class="hero-subtitle">
              Gestandaardiseerde audiograminterpretatie voor betere doorverwijzing
            </h2>
            <p class="hero-body">
              Vereenvoudig interpretatie, rapportage en communicatie — zodat zorgprofessionals minder tijd
              kwijt zijn aan data en meer aan de patiënt.
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
              interaction-prompt="none"
              shadow-intensity="0.6"
              environment-image="neutral"
            />
          </div>
        </div>

      </section>
    <main class="main">

      <section class="tagline" aria-label="Tagline">
        <h3 class="tagline-title">Zorgprofessionals verdrinken in data, niet in tijd.</h3>
        <p class="tagline-sub">
          Sonaris helpt interpretatie structureren, zodat beslissingen sneller en consistenter worden.
        </p>
        <div class="tagline-icons" aria-hidden="true">
          <div v-for="icon in featureIcons" :key="icon.id" class="mini-icon" />
        </div>
      </section>

      <section class="highlights" id="over-ons" aria-label="Highlights">
        <div class="highlight" v-for="card in highlightCards" :key="card.title">
          <div class="highlight-copy">
            <h3 class="section-title">{{ card.title }}</h3>
            <p class="section-body">{{ card.body }}</p>
          </div>
          <div class="highlight-media" aria-hidden="true">
            <div class="img-placeholder" />
          </div>
        </div>
      </section>

      <section class="micro" aria-label="Micro features">
        <div class="micro-grid">
          <article v-for="item in microCards" :key="item.id" class="micro-card">
            <div class="micro-icon" aria-hidden="true" />
            <h4 class="micro-title">{{ item.title }}</h4>
            <p class="micro-body">{{ item.body }}</p>
          </article>
        </div>
      </section>

      <section class="bap" id="onderzoek" aria-label="Onderzoek">
        <div class="bap-top">
          <div class="bap-image img-placeholder" aria-hidden="true" />
          <div class="bap-image img-placeholder" aria-hidden="true" />
        </div>
        <div class="bap-center">
          <h3 class="bap-title">BAP</h3>
          <p class="bap-body">
            Van ruwe audiogramdata naar consistente interpretatie en een rapportage die klaar is om te delen.
          </p>
        </div>

        <div class="bap-bottom">
          <div class="stats">
            <div class="stat">
              <div class="stat-kicker">BAMABAMBAMBA</div>
              <div class="stat-value">1.5+mld</div>
              <div class="stat-label">audiogrammen wereldwijd</div>
            </div>
            <div class="stat">
              <div class="stat-value">90%+</div>
              <div class="stat-label">rapportage bruikbaar na eerste output</div>
            </div>
            <div class="stat-card img-placeholder" aria-hidden="true" />
          </div>

          <div class="bap-list">
            <article class="bap-item">
              <div class="bap-bullet" aria-hidden="true" />
              <div>
                <h4 class="bap-item-title">Lorem ipsum dolor</h4>
                <p class="bap-item-body">
                  Snelle interpretatie met vaste terminologie — makkelijker vergelijken en doorverwijzen.
                </p>
              </div>
            </article>
            <article class="bap-item">
              <div class="bap-bullet" aria-hidden="true" />
              <div>
                <h4 class="bap-item-title">Lorem ipsum dolor</h4>
                <p class="bap-item-body">
                  Automatische samenvatting in een consistent format voor dossier, verwijzing en overleg.
                </p>
              </div>
            </article>
            <article class="bap-item">
              <div class="bap-bullet" aria-hidden="true" />
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
          <div class="usp-media img-placeholder" aria-hidden="true" />
        </div>
      </section>

      <section class="products" aria-label="Products">
        <h3 class="section-title centered">Our products</h3>
        <div class="product-grid">
          <article v-for="p in products" :key="p.id" class="product-card">
            <div class="product-image img-placeholder" aria-hidden="true" />
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
                <div class="avatar" aria-hidden="true" />
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
            <a class="social-dot" href="#" aria-label="Social 1" />
            <a class="social-dot" href="#" aria-label="Social 2" />
            <a class="social-dot" href="#" aria-label="Social 3" />
            <a class="social-dot" href="#" aria-label="Social 4" />
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
}

.topbar {
  position: sticky;
  top: 0;
  z-index: 10;
  background: color-mix(in srgb, var(--color-background) 92%, transparent);
  backdrop-filter: blur(10px);
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
}

.nav-link:hover {
  text-decoration: underline;
}

.nav-cta {
  text-decoration: none;
  font-size: 12px;
  padding: 8px 12px;
  border-radius: 999px;
  background: #000000;
  color: #ffffff;
}

.hero {
  position: relative;
  overflow: hidden;
  padding: 56px 18px 28px;
  background-image: url('./assets/wave-red.svg');
  background-repeat: no-repeat;
  background-position: 50% 100%;
  background-size: min(1600px, 120vw) auto;
}

.hero-inner {
  max-width: 1080px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1.2fr 0.8fr;
  gap: 28px;
  align-items: center;
}

.hero-title {
  font-size: 44px;
  line-height: 1.05;
  font-weight: 700;
  letter-spacing: -0.02em;
}

.hero-subtitle {
  margin-top: 10px;
  font-size: 14px;
  font-weight: 700;
}

.hero-body {
  margin-top: 10px;
  max-width: 48ch;
  color: color-mix(in srgb, var(--color-text) 75%, transparent);
}

.hero-actions {
  margin-top: 14px;
}

.hero-media {
  display: grid;
  place-items: center;
}

.hero-model {
  width: 320px;
  height: 320px;
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
  background: var(--color-background-mute);
  border-radius: 10px;
  border: 1px solid var(--color-border);
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
  background-image: url('./assets/wave-gray.svg');
  background-repeat: no-repeat;
  background-position: right center;
  background-size: 900px auto;
}

.bap-top {
  max-width: 1080px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 18px;
}

.bap-image {
  aspect-ratio: 16 / 9;
}

.bap-center {
  max-width: 760px;
  margin: 16px auto 0;
  text-align: center;
}

.bap-title {
  font-size: 22px;
  font-weight: 700;
}

.bap-body {
  margin-top: 10px;
  color: color-mix(in srgb, var(--color-text) 70%, transparent);
}

.bap-bottom {
  max-width: 1080px;
  margin: 22px auto 0;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 18px;
  align-items: start;
}

.stats {
  display: grid;
  gap: 12px;
}

.stat {
  padding: 10px 12px;
}

.stat-kicker {
  font-size: 10px;
  letter-spacing: 0.12em;
  opacity: 0.7;
}

.stat-value {
  margin-top: 4px;
  font-size: 26px;
  font-weight: 800;
  color: var(--color-accent);
}

.stat-label {
  margin-top: 4px;
  font-size: 11px;
  color: color-mix(in srgb, var(--color-text) 70%, transparent);
}

.stat-card {
  aspect-ratio: 16 / 9;
}

.bap-list {
  padding-top: 45%;
  display: grid;
  gap: 70px;
}

.bap-item {
  display: grid;
  grid-template-columns: 18px 1fr;
  gap: 10px;
  align-items: start;
}

.bap-bullet {
  width: 14px;
  height: 14px;
  border-radius: 999px;
  background: var(--color-background-mute);
  border: 1px solid var(--color-border);
  margin-top: 4px;
}

.bap-item-title {
  font-size: 12px;
  font-weight: 700;
}

.bap-item-body {
  margin-top: 4px;
  font-size: 11px;
  color: color-mix(in srgb, var(--color-text) 70%, transparent);
}

.usp {
  padding: 18px;
}

.usp-card {
  max-width: 1080px;
  margin: 0 auto;
  background: #7d7d7d7b;
  color: #ffffff;
  border-radius: 14px;
  padding: 18px;
  display: grid;
  grid-template-columns: 1.2fr 0.8fr;
  gap: 18px;
  align-items: center;
}

.usp-title {
  font-weight: 800;
  font-size: 16px;
}

.usp-body {
  margin-top: 6px;
  color: color-mix(in srgb, var(--color-text) 70%, transparent);
}

.usp-media {
  aspect-ratio: 16 / 9;
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
}

.product-image {
  aspect-ratio: 4 / 3;
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
  background: var(--color-background);
  cursor: pointer;
  opacity: 0.75;
  transition: transform 0.2s ease, opacity 0.2s ease;
}

.testimonial.active {
  opacity: 1;
  transform: translateY(-2px);
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
  border: 1px solid var(--color-border);
  background: var(--color-background-mute);
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
  background: var(--color-background);
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
  width: 18px;
  height: 18px;
  border-radius: 999px;
  border: 1px solid color-mix(in srgb, var(--vt-c-white) 60%, transparent);
  background: color-mix(in srgb, var(--vt-c-white) 20%, transparent);
  display: inline-block;
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

@media (max-width: 900px) {
  .hero-inner {
    grid-template-columns: 1fr;
  }

  .highlight {
    grid-template-columns: 1fr;
  }

  .micro-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .bap-top {
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
</style>
