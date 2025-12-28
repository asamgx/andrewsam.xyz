# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build & Development Commands

```bash
# Install dependencies
yarn

# Development server (runs prisma generate first)
yarn dev

# Production build (runs prisma generate, db push, and postbuild script)
yarn build

# Start production server
yarn serve

# Linting
yarn lint

# Bundle analysis
yarn analyze
```

**Windows note**: Run `$env:PWD = $(Get-Location).Path` before `yarn dev` if on Windows.

## Architecture Overview

This is a Next.js 14 personal blog using the App Router with React Server Components. Content is managed via Contentlayer (MDX-based).

### Core Technologies
- **Next.js 14** with App Router and TypeScript
- **Contentlayer2** for MDX content processing (blog posts, authors, resume)
- **Prisma** with MongoDB for user auth, comments, likes, and view tracking
- **Auth.js (NextAuth v5 beta)** for authentication (Google, GitHub, credentials)
- **ShadCN/Radix UI** for component library
- **Tailwind CSS** for styling

### Directory Structure

- `app/` - Next.js App Router pages
  - `(landing)/` - Main public routes (blog, about, projects, resume, tags)
  - `api/` - API routes (auth, blog likes, email, github, spotify, views)
  - `x/` - Protected/experimental section
- `components/` - React components including ShadCN UI components
- `data/` - Site configuration and MDX content
  - `siteMetadata.js` - Main site configuration
  - `mainData.ts` - Projects, technologies, and experience data
  - `navLinks.ts` - Navigation configuration
  - `authors/` - Author MDX files
  - `resume/` - Resume MDX content
- `layouts/` - Page layout templates for posts and author pages
  - `PostLayout.tsx`, `PostSimple.tsx`, `PostBanner.tsx` - Blog post layouts
  - `ListLayout.tsx`, `ListLayoutWithTags.tsx` - Blog listing layouts
- `lib/` - Utilities, hooks, and validation schemas
- `prisma/schema.prisma` - Database schema (MongoDB)

### Content System

Blog content is defined in `contentlayer.config.ts`:
- **Production**: reads from `data/blog/**/*.mdx`
- **Development**: reads from `data/blog_examples/**/*.mdx`
- **Authors**: `data/authors/**/*.mdx`
- **Resume**: `data/resume/**/*.mdx`

Frontmatter fields: `title` (required), `date` (required), `tags`, `lastmod`, `draft`, `hidden`, `summary`, `images`, `thumbnail`, `authors`, `layout`, `canonicalUrl`, `bibliography`

### Path Aliases

```typescript
@/components/* -> components/*
@/data/*       -> data/*
@/layouts/*    -> layouts/*
@/css/*        -> css/*
@/*            -> ./*
contentlayer/generated -> .contentlayer/generated
```

### Environment Variables

Required for full functionality (see `.env.example`):
- `DATABASE_URL` - MongoDB connection string
- `NEXTAUTH_URL`, `NEXTAUTH_SECRET` - Auth configuration
- OAuth providers: `GOOGLE_CLIENT_*`, `GITHUB_CLIENT_*`
- `UPSTASH_REDIS_*` - Rate limiting
- `RESEND_TOKEN` - Email functionality
- `SPOTIFY_*` - Spotify integration
- `NEXT_PUBLIC_GISCUS_*` - Comments system

### Key Integrations

- **Giscus** for comments (configured in `siteMetadata.js`)
- **Umami** for analytics
- **Spotify** API for now-playing widget
- **GitHub** API for contribution data
- **Resend** for transactional emails
