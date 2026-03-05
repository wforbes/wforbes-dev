
*Initial Prompt*

I'm a Senior Software Engineer with just over 8 years of experience. I'd like to work with you in creating a website to help support my career going forward. My updated resume is at @wforbes_fullstack_resume_2026.pdf . I've been working at the same company for about 3 years now and I'd like start hitting the job market to find a higher paying job. To help that job search I enrolled in a career coaching program recently named 'Devlaunch' ( https://training.devlaunch.us/ ), and one of their main focuses is dialing down my job application process and building out this new resume. A major component of building up my online presence is building up a solid website that can act as a landing page for anyone who searches information about me.

The website doesn't need to be extremely flashy or large in scope. I don't have much in the way of 'finished' portfolio projects that I can display yet, though I hope to turn my focus in that direction soon. I'm interested in working with you to build up the design concepts around this starting point and move on from there. I think it may be useful to provide some sort of 'blogging' on this website, being able to craft articles related to software engineering, tech industry trends, and other professional topics. I am open to ideas for any other types of content or structures for the website as well.

A career niche that I would like to begin concentrating on is 'AI Engineering', that is - the development of applications that leverage foundation models to provide AI powered features to users. I can see this is a growing market trend in the tech industry and I would like to be a part of it. I have not worked with anything like that professionally yet, but I expect that the portfolio projects I work on this year will seek to demonstrate these skills as I learn more about them.

Aside from this markdown document and the resume PDF file I mentioned, this is a completely empty project folder. We're starting from the ground up. Please review my resume and based on the content there - let's start discussing, designing, and planning my website. The domain this website will be hosted on is https://wforbes.dev

I'm not active on them, but here are my social profiles:
- https://linkedin.com/in/wforbes87
- https://bsky.app/profile/wforbes.dev : just started this profile tonight
- https://x.com/wforbes87
- https://medium.com/@wforbes
- https://www.instagram.com/wforbes87/ : mostly posted skateboarding related content here, so I may start a new instagram profile for professional software engineering content instead
- I have a personal facebook profile, but I expect I may create a Facebook 'Page' for my professional software engineering content if I do use that website for this effort

---

# wforbes.dev — Project Architecture & Implementation Plan

## Context

Will Forbes is a Senior Full Stack Engineer building a personal website at **wforbes.dev** to support his job search and establish professional online presence. The site serves as a landing page for recruiters/hiring managers and a platform for technical blogging. The site should be clean, professional, and extensible — with room to add portfolio projects later as he builds them out.

## Tech Stack

| Layer       | Choice              | Rationale |
|-------------|---------------------|-----------|
| Framework   | **Astro**           | Content-focused, ships minimal JS, first-class MDX support |
| Styling     | **Tailwind CSS**    | Utility-first, fast to build, widely adopted |
| Blog        | **MDX + Astro Content Collections** | Version-controlled, no external dependencies |
| Language    | **TypeScript**      | Type safety, aligns with Will's professional stack |
| Runtime     | **Bun**             | Fast JS runtime and package manager, single tool for install/run/build |
| Deployment  | **Coolify + Docker + Nginx** | Leverages existing Hostinger VPS and Coolify instance |

## Site Structure (Pages)

```
/                   → Landing page (hero, summary, highlights, social links)
/about              → Detailed background, career narrative, downloadable resume
/blog               → Blog listing (articles with date, tags, excerpts)
/blog/[slug]        → Individual blog post (MDX rendered)
/contact            → Contact information and social links
```

**Future additions (not in initial build):**
- `/projects` — Portfolio project showcase

## Project File Structure

```
wforbes-dev/
├── src/
│   ├── components/          # Reusable UI components
│   │   ├── Header.astro     # Site navigation header
│   │   ├── Footer.astro     # Site footer with social links
│   │   ├── Hero.astro       # Landing page hero section
│   │   ├── BlogCard.astro   # Blog post preview card
│   │   ├── TagList.astro    # Tag display component
│   │   └── SocialLinks.astro # Social media icon links
│   ├── layouts/
│   │   ├── BaseLayout.astro # Root layout (head, meta, header, footer)
│   │   └── BlogPost.astro   # Blog post layout (title, date, tags, content)
│   ├── pages/
│   │   ├── index.astro      # Home / landing page
│   │   ├── about.astro      # About page
│   │   ├── contact.astro    # Contact page
│   │   └── blog/
│   │       ├── index.astro        # Blog listing page
│   │       └── [...slug].astro    # Dynamic blog post route
│   ├── content/
│   │   ├── config.ts        # Content collection schema definitions
│   │   └── blog/            # MDX blog posts live here
│   │       └── hello-world.mdx    # Starter post
│   └── styles/
│       └── global.css       # Tailwind directives + global styles
├── public/
│   ├── favicon.svg          # Site favicon
│   ├── wforbes_fullstack_resume_2026.pdf  # Downloadable resume
│   └── images/              # Static images (headshot, etc.)
├── Dockerfile               # Multi-stage: build Astro, serve with Nginx
├── .dockerignore             # Exclude node_modules, .git, etc.
├── nginx.conf               # Nginx config for serving static files
├── astro.config.mjs         # Astro configuration
├── tailwind.config.mjs      # Tailwind configuration
├── tsconfig.json            # TypeScript configuration
├── package.json
├── .gitignore
└── design.md                # This design document
```

## Page Design Concepts

### Home Page (`/`)
- **Hero section**: Name, title ("Backend-Focused Full Stack Engineer"), brief tagline, and CTA (view blog, contact)
- **Highlights section**: 3-4 key career accomplishments pulled from resume (e.g., "Consolidated 23 microservices", "Saved $20K/mo in hosting costs")
- **Latest posts**: Show 2-3 most recent blog posts
- **Tech focus callout**: Brief mention of AI Engineering interest/direction

### About Page (`/about`)
- Professional narrative (more personal than a resume)
- Career timeline or key experience highlights
- Education
- Link to download resume PDF
- Security clearance mention

### Blog Listing (`/blog`)
- Cards showing title, date, excerpt, tags
- Filter by tag (client-side, no JS framework needed — Astro can generate tag pages statically)

### Blog Post (`/blog/[slug]`)
- Clean reading experience with good typography
- Post metadata: date, reading time (calculated), tags
- MDX support allows embedding custom components (code blocks, callouts, etc.)

### Contact (`/contact`)
- Social links (LinkedIn, Bluesky, X, Medium, GitHub, Instagram)
- Email contact
- Simple and direct — no contact form needed initially

## Blog Content Collection Schema

```typescript
// src/content/config.ts
import { defineCollection, z } from 'astro:content';

const blog = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    description: z.string(),
    pubDate: z.coerce.date(),
    updatedDate: z.coerce.date().optional(),
    tags: z.array(z.string()).default([]),
    draft: z.boolean().default(false),
    heroImage: z.string().optional(),
  }),
});

export const collections = { blog };
```

## Visual Design Direction

- **Theme**: Clean, modern, professional — dark mode by default with light mode toggle
- **Typography**: System font stack or a clean sans-serif (Inter, or similar)
- **Color palette**: Neutral base with a single accent color (blue tones pair well with tech/engineering)
- **Spacing**: Generous whitespace, not cramped
- **Responsive**: Mobile-first, fully responsive

## Deployment Architecture (Coolify + Docker)

```
GitHub repo → Coolify detects push → Docker build (multi-stage) → Nginx serves static files
```

### Dockerfile (multi-stage build)
1. **Build stage**: `oven/bun` image, `bun install`, run `bun run build`
2. **Serve stage**: Nginx alpine image, copy `dist/` from build stage, apply nginx.conf

### Coolify Setup
- Point Coolify at the GitHub repo
- Coolify auto-detects the Dockerfile and builds on push to `main`
- Configure custom domain `wforbes.dev` in Coolify
- Coolify handles SSL via Let's Encrypt automatically
- No GitHub Actions needed — Coolify is the entire CI/CD pipeline

### Nginx Configuration
- Serve static files from `/usr/share/nginx/html`
- SPA-style fallback for clean URLs
- Caching headers for static assets (images, CSS, JS)
- Gzip compression enabled

## Implementation Steps

### Phase 1: Project Scaffolding
1. Initialize Astro project with TypeScript
2. Install and configure Tailwind CSS integration
3. Install and configure MDX integration
4. Set up base layout and global styles
5. Set up `.gitignore`

### Phase 2: Core Pages
6. Build `Header` and `Footer` components
7. Build the home page (`/`) with hero, highlights, and latest posts sections
8. Build the about page (`/about`)
9. Build the contact page (`/contact`)

### Phase 3: Blog System
10. Define blog content collection schema
11. Build blog listing page (`/blog`)
12. Build blog post layout and dynamic route (`/blog/[slug]`)
13. Create a starter "Hello World" blog post in MDX
14. Add tag-based filtering

### Phase 4: Polish
15. Add dark/light mode toggle
16. Add meta tags and Open Graph data for SEO
17. Add favicon and any static assets
18. Responsive design pass

### Phase 5: Deployment
19. Create `Dockerfile` (multi-stage: build Astro + serve with Nginx)
20. Create `.dockerignore`
21. Create `nginx.conf` for static file serving
22. Push to GitHub, configure Coolify to deploy from repo
23. Set custom domain and SSL in Coolify

## Verification

- `bun run dev` — local dev server runs without errors
- `bun run build` — static build completes successfully
- `docker build` — container builds without errors
- All pages render correctly at their routes
- Blog posts render from MDX content
- Responsive layout works on mobile/tablet/desktop
- Lighthouse audit scores well for Performance, SEO, Accessibility