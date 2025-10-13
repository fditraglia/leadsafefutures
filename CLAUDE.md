# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based static website for the ECLIPS Project (Elevated Childhood Lead Interagency Prevalence Study), a UKRI-funded research initiative led by Northumbria University. The site provides information about childhood lead exposure research, study activities, team members, and news updates.

## Site Architecture

### Jekyll Structure

- **`_config.yml`**: Jekyll site configuration using the Minima theme
- **`index.md`**: Homepage with project overview and ECLIPS branding
- **`_pages/`**: Static pages (activities, contact, FAQ, information, news, team)
- **`_posts/`**: Blog posts with date-prefixed filenames (YYYY-MM-DD-Title.md)

### Content Organization

**Pages** (`_pages/` directory):
- Each page has YAML front matter with `layout`, `title`, and `permalink`
- Pages are included via `include: ['_pages']` in `_config.yml`
- Standard permalinks: `/activities/`, `/contact/`, `/faq/`, `/information/`, `/news/`, `/team/`

**Blog Posts** (`_posts/` directory):
- Follow Jekyll naming convention: `YYYY-MM-DD-Title.md`
- Front matter includes `layout: post`, `title`, and `date`
- Displayed on the `/news/` page using Jekyll's `site.posts` collection
- News page uses Liquid templating to auto-generate post listings with excerpts

**Images**:
- Currently hosted on GitHub user-attachments
- Referenced directly in markdown files
- ECLIPS logo and team photos embedded throughout

## Common Development Tasks

### Local Development

Jekyll is a Ruby-based static site generator. To run locally:

```bash
# Install dependencies (first time only)
bundle install

# Serve site locally with live reload
bundle exec jekyll serve

# Build site to _site/ directory
bundle exec jekyll build

# Serve with drafts visible
bundle exec jekyll serve --drafts
```

Site will be available at `http://localhost:4000`

### Creating New Content

**New Blog Post**:
1. Create file in `_posts/` with format: `YYYY-MM-DD-post-title.md`
2. Add front matter:
```yaml
---
layout: post
title: "Post Title"
date: YYYY-MM-DD
---
```
3. Write content in Markdown
4. Post will automatically appear on `/news/` page

**New Page**:
1. Create file in `_pages/` directory
2. Add front matter with permalink:
```yaml
---
layout: page
title: Page Title
permalink: /page-url/
---
```
3. Write content in Markdown

### Content Guidelines

- **Images**: Currently using GitHub-hosted images via user-attachments. Consider migrating to local `/assets/images/` directory for better control
- **Logo**: ECLIPS logo (ECLIPS-Colour-6) appears on multiple pages with varying sizes (300px, 150px)
- **Front Matter**: All content files require YAML front matter with layout, title, and permalink/date
- **Navigation**: Pages appear in site navigation based on Jekyll theme's automatic menu generation

## Site Deployment

This site uses GitHub Pages (indicated by `CNAME` file). Deployment is automatic:
- Push to main branch triggers Jekyll build on GitHub Pages
- Custom domain configured in `CNAME` file
- No manual build/deploy steps required

## Important Notes

- **Theme**: Uses Jekyll Minima theme (specified in `_config.yml`)
- **Git Ignored**: `_site/`, `.sass-cache/`, `.jekyll-cache/`, `.jekyll-metadata`, `.bundle/`, `vendor/`
- **Content Focus**: Academic/research website focused on childhood lead exposure study
- **Team Pages**: Extensive team member bios with photos on `/team/` page
- **FAQ**: Comprehensive FAQ covering lead exposure health impacts, study methodology, and participation details

## Repository Information

- **License**: Project is licensed (see LICENSE file)
- **Git Branch**: Main branch is `main`
- **Recent Activity**: Updates include WHO information, ECLIPS newsletter content, and FAQ revisions
