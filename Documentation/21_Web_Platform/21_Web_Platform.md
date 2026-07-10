# WEB PLATFORM ARCHITECTURE

## Browser-Based Learning Application

---

| Document ID | IEP-ARCH-WEB-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Web Architecture Team |
| Mobile App Reference | IEP-ARCH-MOB-001 |
| API Reference | IEP-ARCH-API-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. WEB PHILOSOPHY
3. TECHNOLOGY STACK
4. APPLICATION ARCHITECTURE
5. RENDERING STRATEGY
6. STATE MANAGEMENT
7. RESPONSIVE DESIGN
8. PERFORMANCE
9. SEO AND ACCESSIBILITY
10. PWA CAPABILITIES
11. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The Web Platform delivers the Islamic Education Platform through browsers, providing a rich, responsive experience across desktop and tablet devices. It complements the mobile app with enhanced capabilities for content creation, management, and deep study.

### 1.2 Core Principle

**Full-featured, fast, and accessible from any browser.**

---

## 2. WEB PHILOSOPHY

| Principle | Description |
|---|---|
| **Progressive Enhancement** | Core content works without JavaScript; enhanced with JS |
| **Responsive** | Seamless experience from phone to desktop |
| **Accessible** | WCAG 2.1 AA compliance |
| **SEO-Optimized** | Islamic content discoverable via search |
| **PWA-Ready** | Installable, offline-capable, push notifications |
| **SSR-First** | Server-side rendering for performance and SEO |

---

## 3. TECHNOLOGY STACK

| Layer | Technology | Rationale |
|---|---|---|
| **Framework** | Next.js 15+ | SSR, App Router, React Server Components |
| **UI Library** | React 19+ | Component-based, large ecosystem |
| **Styling** | Tailwind CSS 4 + shadcn/ui | Utility-first, design system |
| **State Management** | Zustand + React Query | Lightweight, server state management |
| **Forms** | React Hook Form + Zod | Validation, performance |
| **Testing** | Playwright + Vitest | E2E and unit testing |
| **Analytics** | Plausible / PostHog | Privacy-focused analytics |

---

## 4. APPLICATION ARCHITECTURE

### 4.1 Next.js Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      CDN (Vercel Edge / Cloudflare)                     │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
┌─────────────────────────────────────────────────────────────────────────┐
│                      NEXT.JS APPLICATION                                │
│                                                                          │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐                    │
│  │ App Router  │  │ API Routes  │  │ Middleware  │                    │
│  │ (RSC)       │  │ (Serverless)│  │ (Edge)      │                    │
│  └─────────────┘  └─────────────┘  └─────────────┘                    │
│                                                                          │
│  ┌─────────────────────────────────────────────────────┐               │
│  │  Server Components (RSC)       Client Components     │               │
│  │  • Data fetching               • Interactivity      │               │
│  │  • SEO content                 • User events         │               │
│  │  • Static rendering            • Browser APIs        │               │
│  └─────────────────────────────────────────────────────┘               │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
┌─────────────────────────────────────────────────────────────────────────┐
│                      BACKEND SERVICES (API)                             │
└─────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Route Structure

| Route Group | Routes | Rendering |
|---|---|---|
| **Public** | `/`, `/about`, `/pricing` | Static (SSG) |
| **Content** | `/learn/[topic]`, `/stories/[id]`, `/podcasts/[id]` | ISR |
| **Learning** | `/dashboard`, `/flashcards`, `/quizzes` | SSR + Client |
| **Auth** | `/login`, `/register`, `/reset-password` | Client |
| **Admin** | `/admin/*` | SSR (protected) |
| **API** | `/api/*` | Serverless |

---

## 5. RENDERING STRATEGY

| Strategy | Use Case | TTFB | Freshness |
|---|---|---|---|
| **Static (SSG)** | Public pages, content pages | Fastest | On build |
| **ISR** | Content pages with updates | Fast | Configurable revalidation |
| **SSR** | Personalized pages | Moderate | Per request |
| **CSR** | Highly interactive features | Depends on API | Per request |
| **Streaming** | Large content pages | First paint fast | Progressive loading |

---

## 6. STATE MANAGEMENT

| State Type | Tool | Persistence |
|---|---|---|
| **Server State** | React Query (TanStack Query) | Cache + background refresh |
| **Client State** | Zustand | Session (or localStorage) |
| **Form State** | React Hook Form | Local component |
| **URL State** | Next.js searchParams | URL |
| **Auth State** | NextAuth.js / JWT | HTTP-only cookie + memory |

---

## 7. RESPONSIVE DESIGN

| Breakpoint | Width | Target | Layout |
|---|---|---|---|
| **Mobile** | < 640px | Smartphones | Single column, bottom nav |
| **Tablet** | 640-1024px | iPads, tablets | Two column, side nav collapsible |
| **Desktop** | 1024-1440px | Laptops | Multi-column, side nav |
| **Wide** | > 1440px | Large monitors | Max-width container, enhanced |

---

## 8. PERFORMANCE

| Metric | Target |
|---|---|
| **LCP** | < 2.5 seconds |
| **FID / INP** | < 100ms |
| **CLS** | < 0.1 |
| **First Load JS** | < 200 KB |
| **Lighthouse Score** | > 90 all categories |
| **Time to Interactive** | < 3 seconds |
| **Core Web Vitals Pass** | 100% of pages |

---

## 9. SEO AND ACCESSIBILITY

### 9.1 SEO Strategy

| Element | Implementation |
|---|---|
| **Metadata** | Dynamic `generateMetadata` with Arabic support |
| **Structured Data** | JSON-LD for Islamic content (Quran, Hadith, educational) |
| **Sitemap** | Dynamic sitemap for all content pages |
| **RSS Feed** | Podcast and content feeds |
| **Multi-Language** | hreflang tags for all supported languages |
| **Arabic SEO** | Arabic URL slugs, proper Arabic meta tags |

### 9.2 Accessibility

| Standard | Target | Implementation |
|---|---|---|
| **WCAG 2.1** | Level AA | Semantic HTML, ARIA labels, focus management |
| **Screen Reader** | VoiceOver, TalkBack, NVDA | Proper heading hierarchy, alt text, landmarks |
| **Keyboard Navigation** | Full keyboard support | Focus indicators, skip links, tab order |
| **RTL Support** | Full RTL layout | dir="rtl", logical CSS properties |
| **Color Contrast** | 4.5:1 (normal), 3:1 (large) | Design system enforcement |
| **Font Scaling** | Up to 200% | Responsive typography |

---

## 10. PWA CAPABILITIES

| Feature | Implementation |
|---|---|
| **Service Worker** | Workbox for caching strategies |
| **Offline Page** | Custom offline experience with cached content |
| **Install Prompt** | Custom install button for Android/Chrome |
| **Push Notifications** | Web Push API |
| **Background Sync** | Sync offline progress when online |
| **App Shell** | Minimal shell cached for instant loading |

---

## 11. APPENDICES

### 11.1 Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| IEP-ARCH-MOB-001 | Mobile Application | Cross-platform design system |
| IEP-ARCH-API-001 | API Integration | Backend communication |
| IEP-ARCH-SEC-001 | Security Architecture | Web security |

### 11.2 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-07 | Office of the Chief Architect | Initial approved release |
