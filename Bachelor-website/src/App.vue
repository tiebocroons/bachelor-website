<script setup>
import { computed, onBeforeUnmount, onMounted, reactive, ref } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import heroModelSrc from './assets/model.glb?url'
import heroWaveSrc from './assets/wave-red.svg?url'

const navLinks = [
  { label: 'About', href: '#about' },
  { label: 'Research', href: '#research' },
  { label: 'Contact', href: '#contact' },
]

const highlightCards = [
  {
    title: 'What is Sonaris?',
    body: 'Sonaris is a digital application that supports healthcare professionals in analyzing and interpreting audiograms. By automatically structuring complex measurement data and translating it into clear, evidence-based classifications, Sonaris helps teams reach consistent and transparent insights faster. The tool acts as decision support: it strengthens clinical judgment without replacing it.',
    image: '/medel_kindergarten_raw_danielzangerl%20(91%20von%20091)%20copy.webp',
  },
  {
    title: 'Why Sonaris matters',
    body: 'This is especially relevant for audiologists, ENT specialists, and other hearing care professionals who face increasing time pressure, growing volumes of data, and the need for consistent interpretation. Sonaris supports more efficient workflows, reduces variation in interpretation, and contributes to high-quality, ethically responsible care.',
    image: '/vibrant_familie_danielzangerl%20(1%20von%20001)%20als%20Smartobjekt-1.webp',
  },
]

const products = [
  {
    id: 1,
    title: 'Interpretation support',
    body: 'Turn raw audiogram data into structured, clinically useful insights in less time.',
    image: '/Portrait%20of%20a%20lady%20with%20a%20cochlear%20implant..jpg',
  },
  {
    id: 2,
    title: 'Consistent reporting',
    body: 'Generate a clear summary format for records, referrals, and multidisciplinary review.',
    image: '/_24A0522.jpg',
  },
  {
    id: 3,
    title: 'Referral guidance',
    body: 'Highlight when further evaluation for hearing aids or cochlear implants should be considered.',
    image: '/medel_kindergarten_raw_danielzangerl%20(12%20von%20012).webp',
  },
  {
    id: 4,
    title: 'Workflow efficiency',
    body: 'Reduce repetitive interpretation steps so more attention stays with the patient.',
    image: '/kopf+AP.jpg',
  },
]

const testimonials = [
  {
    id: 1,
    rating: 5,
    quote:
      '"Strong impact in a single day: clear insights, less noise, and immediately useful in practice."',
    name: 'Dr. Sophie van den Berg',
    role: 'ENT specialist',
    avatar: '/_24A0522.jpg',
  },
  {
    id: 2,
    rating: 5,
    quote:
      '"The combination of interpretation and reporting makes referrals faster and more consistent."',
    name: 'A. de Vries',
    role: 'Hearing care specialist',
    avatar: '/Portrait%20of%20a%20lady%20with%20a%20cochlear%20implant..jpg',
  },
  {
    id: 3,
    rating: 5,
    quote:
      '"Fewer administrative steps, more focus on the patient. Exactly what we need."',
    name: 'M. Jansen',
    role: 'Healthcare professional',
    avatar: '/medel_familie_logopaedie_2014_danielzangerl_raw%20(68%20von%20068).webp',
  },
]

// ── Contact form ─────────────────────────────────────────────────────────────
// 1. Go to https://formspree.io and create a free account.
// 2. Create a new form and set the destination email to sonaris@lukasdhaese.be.
// 3. Copy the form ID from your dashboard (e.g. "xrgvaklp") and replace below.
const FORMSPREE_ENDPOINT = 'https://formspree.io/f/mzdjywap'

const contactForm = reactive({
  name: '',
  organization: '',
  role: '',
  email: '',
  message: '',
})
const formStatus = ref('idle') // 'idle' | 'submitting' | 'success' | 'error'

async function submitContactForm() {
  formStatus.value = 'submitting'
  try {
    const res = await fetch(FORMSPREE_ENDPOINT, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', Accept: 'application/json' },
      body: JSON.stringify(contactForm),
    })
    if (res.ok) {
      formStatus.value = 'success'
      Object.assign(contactForm, { name: '', organization: '', role: '', email: '', message: '' })
    } else {
      formStatus.value = 'error'
    }
  } catch {
    formStatus.value = 'error'
  }
}
// ─────────────────────────────────────────────────────────────────────────────

const activeTestimonial = ref(1)
const pageRef = ref(null)
const isMobileMenuOpen = ref(false)

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
  window.addEventListener('resize', handleViewportChange)
  handleViewportChange()

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
          trigger: '.contact-section',
          start: 'top 84%',
          once: true,
        },
      })
      .from('.contact-section .section-title, .contact-body', {
        ...textReveal,
        stagger: 0.1,
      })
      .from(
        '.contact-form',
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
  window.removeEventListener('resize', handleViewportChange)
  closeMobileMenu()
  animationContext?.revert()
})

function setMobileMenuState(nextState) {
  isMobileMenuOpen.value = nextState
  document.body.style.overflow = nextState ? 'hidden' : ''
}

function toggleMobileMenu() {
  setMobileMenuState(!isMobileMenuOpen.value)
}

function closeMobileMenu() {
  setMobileMenuState(false)
}

function handleViewportChange() {
  if (window.innerWidth > 600) {
    closeMobileMenu()
  }
}

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
  closeMobileMenu()
  const id = href?.startsWith('#') ? href.slice(1) : href
  const el = document.getElementById(id)
  el?.scrollIntoView({ behavior: 'smooth', block: 'start' })
}
</script>

<template>
  <div ref="pageRef" class="page" id="home">
    <header class="topbar" :class="{ 'menu-open': isMobileMenuOpen }">
      <a class="nav-logo" href="#home" @click.prevent="scrollToAnchor('#home')">
        <img src="/logo_sonaris_red.svg" alt="Sonaris" class="nav-logo-img" />
      </a>

      <button
        class="nav-toggle"
        type="button"
        aria-controls="mobile-menu-popup"
        :aria-expanded="String(isMobileMenuOpen)"
        :aria-label="isMobileMenuOpen ? 'Close menu' : 'Open menu'"
        @click="toggleMobileMenu"
      >
        <span class="nav-toggle-line" aria-hidden="true"></span>
        <span class="nav-toggle-line" aria-hidden="true"></span>
        <span class="nav-toggle-line" aria-hidden="true"></span>
      </button>

      <nav class="nav nav-desktop">
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

      <a class="nav-cta nav-cta-desktop" href="#contact" @click.prevent="scrollToAnchor('#contact')">
        Book a demo
      </a>
      
    </header>

    <div
      id="mobile-menu-popup"
      class="mobile-menu-popup"
      :class="{ 'is-open': isMobileMenuOpen }"
      :aria-hidden="String(!isMobileMenuOpen)"
      @click.self="closeMobileMenu"
    >
      <div class="mobile-menu-surface">
        <p class="mobile-menu-kicker">Menu</p>
        <nav class="mobile-menu-nav" aria-label="Mobile navigation">
          <a
            v-for="(link, index) in navLinks"
            :key="`mobile-${link.href}`"
            class="mobile-menu-link"
            :style="{ '--menu-index': index }"
            :href="link.href"
            @click.prevent="scrollToAnchor(link.href)"
          >
            {{ link.label }}
          </a>
        </nav>

        <a class="mobile-menu-cta" href="#contact" @click.prevent="scrollToAnchor('#contact')">
          Book a demo
        </a>
      </div>
    </div>

    <section class="hero" aria-label="Hero">
        <img class="hero-wave" :src="heroWaveSrc" aria-hidden="true" />
        <div class="hero-inner">
          <div class="hero-copy">
            <h1 class="hero-subtitle">
              Smarter hearing test interpretation for choosing between hearing aids and cochlear implants
            </h1>
            <p class="hero-body">
              Sonaris analyzes audiogram results and translates them into clear recommendations, helping healthcare professionals decide more quickly and confidently which solution best fits each patient.
            </p>

            <div class="hero-actions">
              <a class="btn btn-primary" href="#contact" @click.prevent="scrollToAnchor('#contact')">
                Book a demo
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

      <section class="highlights" id="about" aria-label="Highlights">
        <div class="highlight" v-for="card in highlightCards" :key="card.title">
          <div class="highlight-copy">
            <h3 class="section-title">{{ card.title }}</h3>
            <p class="section-body">{{ card.body }}</p>
          </div>
          <div class="highlight-media" aria-hidden="true">
            <img :src="card.image" alt="Highlight Image" class="img-placeholder" />
          </div>
        </div>
      </section>

      <section class="bap" id="research" aria-label="Research">
        <div class="bap-bottom">
          <div class="stats">
            <div class="stat-kicker">Market context</div>
            <div class="stats-grid">
              <div class="stat">
                <div class="stat-value" data-count-to="1.5" data-decimals="1" data-suffix="+bn">
                  1.5+bn
                </div>
                <div class="stat-label">People worldwide are affected by disabling hearing loss.</div>
              </div>
              <div class="stat">
                <div class="stat-value" data-count-to="90" data-decimals="0" data-suffix="%+">
                  90%+
                </div>
                <div class="stat-label">Of people who medically qualify for a cochlear implant are currently undertreated.</div>
              </div>
            </div>
            <img src="/_24A0522.jpg" alt="Stat Image" class="stat-card" aria-hidden="true" />
          </div>

          <div class="bap-list">
            <article class="bap-item">
              <img src="/kopf+AP.jpg" alt="Bap Item Image" class="bap-item-image" aria-hidden="true" />
              <div>
                <h4 class="bap-item-title">Faster interpretation</h4>
                <p class="bap-item-body">
                  Faster interpretation with consistent terminology for easier comparison and referral decisions.
                </p>
              </div>
            </article>
            <article class="bap-item">
              <img src="/medel_kindergarten_raw_danielzangerl%20(17%20von%20017)%20copy.webp" alt="Bap Item Image" class="bap-item-image" aria-hidden="true" />
              <div>
                <h4 class="bap-item-title">Consistent summaries</h4>
                <p class="bap-item-body">
                  Automatic summaries in a consistent format for records, referrals, and clinical discussion.
                </p>
              </div>
            </article>
            <article class="bap-item">
              <img src="/medel_kindergarten_raw_danielzangerl%20(24%20von%20024)%20copy.webp" alt="Bap Item Image" class="bap-item-image" aria-hidden="true" />
              <div>
                <h4 class="bap-item-title">Relevant output</h4>
                <p class="bap-item-body">
                  Clear output focused on clinical relevance, so decisions can be made more quickly.
                </p>
              </div>
            </article>
          </div>
        </div>
      </section>

      <section class="usp" aria-label="Value proposition">
        <div class="usp-card">
          <div>
            <h3 class="usp-title">Built for confident decisions</h3>
            <p class="usp-body">
              Sonaris combines interpretation support and structured output to help professionals act with more confidence.
            </p>
            <a class="btn btn-dark" href="#contact" @click.prevent="scrollToAnchor('#contact')">Read more</a>
          </div>
          <img src="/vibrant_familie_danielzangerl%20(1%20von%20001)%20als%20Smartobjekt-1.webp" alt="USP Image" class="usp-media" aria-hidden="true" />
        </div>
      </section>

      <section class="products" aria-label="Products">
        <h3 class="section-title centered">Core capabilities</h3>
        <div class="product-grid">
          <article v-for="p in products" :key="p.id" class="product-card">
            <img :src="p.image" alt="Product Image" class="product-image" aria-hidden="true" />
            <h4 class="product-title">{{ p.title }}</h4>
            <p class="product-body">{{ p.body }}</p>
            <a class="btn btn-dark" href="#contact" @click.prevent="scrollToAnchor('#contact')">Read more</a>
          </article>
        </div>
      </section>

      <section class="verdict" aria-label="Testimonials">
        <h3 class="section-title centered">What professionals say</h3>

        <div class="carousel">
          <button class="carousel-arrow" type="button" @click="prevTestimonial" aria-label="Previous">
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
                <img :src="t.avatar" alt="Testimonial Avatar" class="avatar" aria-hidden="true" />
                <div>
                  <div class="person-name">{{ t.name }}</div>
                  <div class="person-role">{{ t.role }}</div>
                </div>
              </div>
            </article>
          </div>

          <button class="carousel-arrow" type="button" @click="nextTestimonial" aria-label="Next">
            ›
          </button>
        </div>
      </section>

      <section class="contact-section" id="contact" aria-label="Book a demo">
        <div class="contact-inner">
          <div class="contact-copy">
            <h3 class="section-title">Book a demo</h3>
            <p class="contact-body">
              Interested in seeing Sonaris in action? Fill in your details and we'll get back to you within 2 business days.
            </p>
          </div>
          <form class="contact-form" @submit.prevent="submitContactForm" novalidate>
            <div v-if="formStatus === 'success'" class="form-success">
              <p>Thank you — we'll be in touch shortly.</p>
            </div>
            <template v-else>
              <div class="form-row">
                <div class="form-group">
                  <label class="form-label" for="cf-name">Name <span aria-hidden="true">*</span></label>
                  <input class="input" id="cf-name" type="text" v-model="contactForm.name" autocomplete="name" placeholder="Dr. Jane Smith" required />
                </div>
                <div class="form-group">
                  <label class="form-label" for="cf-org">Organization</label>
                  <input class="input" id="cf-org" type="text" v-model="contactForm.organization" autocomplete="organization" placeholder="Hospital / Clinic" />
                </div>
              </div>
              <div class="form-row">
                <div class="form-group">
                  <label class="form-label" for="cf-role">Role</label>
                  <select class="input" id="cf-role" v-model="contactForm.role">
                    <option value="" disabled>Select your role</option>
                    <option>Audiologist</option>
                    <option>ENT specialist</option>
                    <option>Hearing care specialist</option>
                    <option>Researcher</option>
                    <option>Other</option>
                  </select>
                </div>
                <div class="form-group">
                  <label class="form-label" for="cf-email">Email <span aria-hidden="true">*</span></label>
                  <input class="input" id="cf-email" type="email" v-model="contactForm.email" autocomplete="email" placeholder="you@hospital.com" required />
                </div>
              </div>
              <div class="form-group">
                <label class="form-label" for="cf-message">Message <span class="optional">(optional)</span></label>
                <textarea class="input textarea" id="cf-message" v-model="contactForm.message" placeholder="Tell us about your use case or any questions you have." rows="4"></textarea>
              </div>
              <div class="form-footer-row">
                <p v-if="formStatus === 'error'" class="form-error" role="alert">
                  Something went wrong. Please try again or email
                  <a href="mailto:sonaris@lukasdhaese.be">sonaris@lukasdhaese.be</a>.
                </p>
                <button class="btn btn-primary" type="submit" :disabled="formStatus === 'submitting'">
                  {{ formStatus === 'submitting' ? 'Sending…' : 'Book a demo' }}
                </button>
              </div>
            </template>
          </form>
        </div>
      </section>
    </main>

    <footer class="footer" aria-label="Footer">
      <div class="footer-inner">
        <div class="footer-top">
          <div class="footer-left">
            <a class="footer-logo-link" href="#home" @click.prevent="scrollToAnchor('#home')">
              <img src="/logo_sonaris_red.svg" alt="Sonaris" class="footer-logo" />
            </a>
            <p class="footer-tagline">Audiogram interpretation for the modern healthcare professional.</p>
            <div class="footer-social" aria-label="Social links">
              <a class="social-dot" href="#" aria-label="Facebook">
                <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M18 2h-3a5 5 0 0 0-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 0 1 1-1h3z"/></svg>
              </a>
              <a class="social-dot" href="#" aria-label="YouTube">
                <svg xmlns="http://www.w3.org/2000/svg" width="13" height="13" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M22.54 6.42a2.78 2.78 0 0 0-1.95-1.96C18.88 4 12 4 12 4s-6.88 0-8.59.46A2.78 2.78 0 0 0 1.46 6.42 29 29 0 0 0 1 12a29 29 0 0 0 .46 5.58 2.78 2.78 0 0 0 1.95 1.96C5.12 20 12 20 12 20s6.88 0 8.59-.46a2.78 2.78 0 0 0 1.95-1.96A29 29 0 0 0 23 12a29 29 0 0 0-.46-5.58z"/><polygon points="9.75 15.02 15.5 12 9.75 8.98 9.75 15.02" fill="white"/></svg>
              </a>
              <a class="social-dot" href="#" aria-label="X (Twitter)">
                <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
              </a>
              <a class="social-dot" href="#" aria-label="Instagram">
                <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><rect x="2" y="2" width="20" height="20" rx="5" ry="5"/><path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"/><line x1="17.5" y1="6.5" x2="17.51" y2="6.5"/></svg>
              </a>
            </div>
          </div>

          <div class="footer-cols">
            <div class="footer-col">
              <div class="footer-heading">Product</div>
              <a class="footer-link" href="#research" @click.prevent="scrollToAnchor('#research')">Research</a>
              <a class="footer-link" href="#about" @click.prevent="scrollToAnchor('#about')">About</a>
            </div>
            <div class="footer-col">
              <div class="footer-heading">Resources</div>
              <a class="footer-link" href="#">Privacy</a>
              <a class="footer-link" href="#">Terms</a>
            </div>
            <div class="footer-col">
              <div class="footer-heading">Contact</div>
              <a class="footer-link" href="mailto:sonaris@lukasdhaese.be">sonaris@lukasdhaese.be</a>
              <a class="footer-link" href="#contact" @click.prevent="scrollToAnchor('#contact')">Get in touch</a>
            </div>
          </div>
        </div>

        <div class="footer-bottom">
          <span class="footer-copy">&copy; 2026 Sonaris. All rights reserved.</span>
          <div class="footer-bottom-links">
            <a class="footer-link-small" href="#">Privacy</a>
            <a class="footer-link-small" href="#">Terms</a>
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

.nav-desktop {
  display: flex;
}

.nav-toggle {
  display: none;
  width: 42px;
  height: 42px;
  border-radius: 12px;
  border: 1px solid #161a1d;
  background: #161a1d;
  box-shadow: 0 10px 20px rgba(22, 26, 29, 0.2);
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap: 4px;
  cursor: pointer;
  transition:
    transform 0.28s cubic-bezier(0.22, 1, 0.36, 1),
    box-shadow 0.28s ease,
    border-color 0.28s ease;
}

.nav-toggle-line {
  width: 18px;
  height: 2px;
  border-radius: 999px;
  background: #ffffff;
  transform-origin: center;
  transition:
    transform 0.32s cubic-bezier(0.22, 1, 0.36, 1),
    opacity 0.22s ease;
}

.topbar.menu-open .nav-toggle-line:nth-child(1) {
  transform: translateY(6px) rotate(45deg);
}

.topbar.menu-open .nav-toggle-line:nth-child(2) {
  opacity: 0;
}

.topbar.menu-open .nav-toggle-line:nth-child(3) {
  transform: translateY(-6px) rotate(-45deg);
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
  display: flex;
  align-items: center;
  text-decoration: none;
}

.nav-logo-img {
  height: 32px;
  width: auto;
  display: block;
}

.nav-cta {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  text-decoration: none;
  font-size: 12px;
  line-height: 1;
  padding: 8px 12px;
  border-radius: 999px;
  background: #000000;
  color: #ffffff;
  transition:
    transform 0.3s cubic-bezier(0.22, 1, 0.36, 1),
    box-shadow 0.3s ease,
    background-color 0.3s ease;
}

.mobile-menu-popup {
  display: none;
  position: fixed;
  inset: 0;
  z-index: 9;
  opacity: 0;
  visibility: hidden;
  pointer-events: none;
  transform: scale(1.03);
  transition:
    opacity 0.34s ease,
    transform 0.5s cubic-bezier(0.22, 1, 0.36, 1),
    visibility 0s linear 0.34s;
  background:
    radial-gradient(110% 80% at 12% 0%, rgba(230, 27, 46, 0.14), transparent 58%),
    radial-gradient(90% 90% at 88% 18%, rgba(255, 211, 216, 0.72), transparent 46%),
    linear-gradient(158deg, #fbf8f6 0%, #f5efec 48%, #efe7e3 100%);
}

.mobile-menu-popup.is-open {
  opacity: 1;
  visibility: visible;
  pointer-events: auto;
  transform: scale(1);
  transition-delay: 0s;
}

.mobile-menu-surface {
  min-height: 100dvh;
  padding: 116px 24px 38px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 22px;
}

.mobile-menu-kicker {
  font-size: 12px;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: rgba(22, 26, 29, 0.45);
  opacity: 0;
  transform: translateY(14px);
  transition:
    opacity 0.34s ease,
    transform 0.42s cubic-bezier(0.22, 1, 0.36, 1);
}

.mobile-menu-nav {
  display: grid;
  gap: 10px;
}

.mobile-menu-link {
  width: fit-content;
  font-size: clamp(2rem, 11vw, 3.5rem);
  line-height: 1;
  font-weight: 800;
  letter-spacing: -0.02em;
  color: #161a1d;
  text-decoration: none;
  opacity: 0;
  transform: translateY(24px);
  transition:
    opacity 0.32s ease,
    transform 0.52s cubic-bezier(0.22, 1, 0.36, 1),
    color 0.2s ease;
  transition-delay: calc(var(--menu-index) * 75ms + 100ms);
}

.mobile-menu-link:hover {
  color: var(--color-accent);
}

.mobile-menu-cta {
  margin-top: 14px;
  width: fit-content;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  height: 44px;
  padding: 0 18px;
  border-radius: 999px;
  border: 1px solid rgba(22, 26, 29, 0.1);
  background: #161a1d;
  color: #ffffff;
  font-size: 13px;
  font-weight: 700;
  text-decoration: none;
  opacity: 0;
  transform: translateY(24px);
  box-shadow: 0 14px 30px rgba(22, 26, 29, 0.14);
  transition:
    opacity 0.32s ease,
    transform 0.52s cubic-bezier(0.22, 1, 0.36, 1),
    background 0.24s ease,
    border-color 0.24s ease;
  transition-delay: 320ms;
}

.mobile-menu-cta:hover {
  background: var(--color-accent);
  border-color: var(--color-accent);
}

.mobile-menu-popup.is-open .mobile-menu-kicker,
.mobile-menu-popup.is-open .mobile-menu-link,
.mobile-menu-popup.is-open .mobile-menu-cta {
  opacity: 1;
  transform: translateY(0);
}

.hero {
  position: relative;
  padding: 120px 18px 80px;
}

.hero-wave {
  position: absolute;
  bottom: -18%;
  /* left: 50%; */
  transform: translateX(-10%);
  width: auto;
  height: auto;
  pointer-events: none;
  z-index: 0;
}

@media (min-width: 1200px) {
  .hero-wave {
    width: 120vw;
  }
}

.hero-inner {
  position: relative;
  z-index: 1;
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

.contact-section {
  padding: 60px 18px 80px;
}

.contact-inner {
  max-width: 1080px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: 60px;
  align-items: start;
}

.contact-copy {
  position: sticky;
  top: 100px;
}

.contact-body {
  margin-top: 10px;
  font-size: 14px;
  line-height: 1.7;
  color: color-mix(in srgb, var(--color-text) 70%, transparent);
  max-width: 34ch;
}

.contact-form {
  display: grid;
  gap: 16px;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 14px;
}

.form-group {
  display: grid;
  gap: 6px;
}

.form-label {
  font-size: 12px;
  font-weight: 600;
  color: color-mix(in srgb, var(--color-text) 75%, transparent);
}

.optional {
  font-weight: 400;
  opacity: 0.6;
}

.textarea {
  height: auto;
  resize: vertical;
  font-family: inherit;
  font-size: inherit;
  line-height: 1.6;
}

.form-footer-row {
  display: flex;
  align-items: center;
  gap: 16px;
  flex-wrap: wrap;
}

.form-error {
  font-size: 12px;
  color: var(--color-accent);
}

.form-error a {
  color: var(--color-accent);
}

.form-success {
  padding: 24px;
  border-radius: 12px;
  background: color-mix(in srgb, var(--color-accent) 8%, transparent);
  border: 1px solid color-mix(in srgb, var(--color-accent) 24%, transparent);
  font-size: 14px;
  font-weight: 600;
  color: var(--color-text);
}

.btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
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
  background: #121417;
  color: #ffffff;
  margin-top: 0;
  position: relative;
  z-index: 2;
}

.footer-inner {
  max-width: 1080px;
  margin: 0 auto;
  padding: 60px 24px 24px;
  z-index: 1000;
}

.footer-top {
  display: flex;
  justify-content: space-between;
  gap: 48px;
  flex-wrap: wrap;
  padding-bottom: 40px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.footer-left {
  display: flex;
  flex-direction: column;
  gap: 14px;
  max-width: 260px;
}

.footer-logo-link {
  display: inline-flex;
}

.footer-logo {
  height: 36px;
  width: auto;
  filter: brightness(0) invert(1);
}

.footer-tagline {
  font-size: 13px;
  line-height: 1.6;
  color: rgba(255, 255, 255, 0.55);
  margin: 0;
}

.footer-social {
  display: flex;
  gap: 8px;
  margin-top: 4px;
}

.social-dot {
  width: 34px;
  height: 34px;
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.12);
  background: rgba(255, 255, 255, 0.06);
  display: inline-flex;
  align-items: center;
  justify-content: center;
  color: rgba(255, 255, 255, 0.7);
  transition:
    background 0.22s ease,
    color 0.22s ease,
    border-color 0.22s ease,
    transform 0.28s cubic-bezier(0.22, 1, 0.36, 1);
}

.social-dot:hover {
  background: var(--color-accent);
  border-color: var(--color-accent);
  color: #ffffff;
  transform: translateY(-2px);
}

.footer-cols {
  display: flex;
  gap: 48px;
  flex-wrap: wrap;
}

.footer-col {
  display: flex;
  flex-direction: column;
  min-width: 110px;
}

.footer-heading {
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.4);
  margin-bottom: 14px;
}

.footer-link {
  display: block;
  color: rgba(255, 255, 255, 0.75);
  text-decoration: none;
  font-size: 13px;
  margin-bottom: 10px;
  transition: color 0.2s ease;
}

.footer-link:hover {
  color: #ffffff;
}

.footer-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 12px;
  padding-top: 20px;
}

.footer-copy {
  font-size: 12px;
  color: rgba(255, 255, 255, 0.35);
}

.footer-bottom-links {
  display: flex;
  gap: 20px;
}

.footer-link-small {
  font-size: 12px;
  color: rgba(255, 255, 255, 0.35);
  text-decoration: none;
  transition: color 0.2s ease;
}

.footer-link-small:hover {
  color: rgba(255, 255, 255, 0.75);
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

  .contact-inner {
    grid-template-columns: 1fr;
  }

  .contact-copy {
    position: static;
  }

  .form-row {
    grid-template-columns: 1fr;
  }
}

/* ── Phone (≤ 600px) ── */
@media (max-width: 600px) {
  .topbar {
    padding: 10px 14px;
    display: grid;
    grid-template-columns: 1fr auto;
    align-items: center;
    gap: 8px;
  }

  .nav-toggle {
    display: inline-flex;
    z-index: 12;
  }

  .nav-desktop,
  .nav-cta-desktop {
    display: none;
  }

  .mobile-menu-popup {
    display: block;
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

  .footer-inner {
    flex-direction: column;
  }

  .footer-cols {
    flex-direction: column;
    gap: 16px;
  }
}
</style>
