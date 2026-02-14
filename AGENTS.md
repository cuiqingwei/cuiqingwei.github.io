# AGENTS.md - Coding Guidelines for hexo-yelee

## Project Overview

This is a **Hexo** static site generator blog using the **Yelee** theme. Hexo is a fast, simple & powerful blog framework powered by Node.js.

- **Hexo Version**: 5.3.0
- **Theme**: Yelee (v3.5)
- **Language**: Chinese (zh-Hans)
- **URL**: http://cuiqingwei.github.io

## Build Commands

```bash
# Generate static files
npm run build
# OR
hexo generate

# Clean generated files
npm run clean
# OR
hexo clean

# Deploy to GitHub Pages
npm run deploy
# OR
hexo deploy

# Start local development server
npm run server
# OR
hexo server

# Create a new post
hexo new post "Post Title"

# Create a new page
hexo new page "Page Title"

# Lint JavaScript files
npm run lint

# Lint and fix JavaScript files
npm run lint:fix

# Format code with Prettier
npm run format

# Check code formatting
npm run format:check
```

## Project Structure

```
/home/gary/Workspace/hexo-yelee/
├── _config.yml          # Site configuration
├── package.json         # Dependencies
├── scaffolds/           # Post templates
│   ├── post.md         # Blog post template
│   ├── page.md         # Page template
│   └── draft.md        # Draft template
├── source/             # Content source
│   ├── _posts/         # Blog posts (markdown files)
│   ├── about/          # About page
│   └── img/            # Static images
└── themes/
    └── yelee/          # Yelee theme
        ├── _config.yml # Theme configuration
        ├── layout/     # EJS templates
        └── source/     # Theme assets
```

## Content Guidelines

### Blog Posts

Posts are stored in `source/_posts/` with the naming convention:

```
YYYY-MM-DD-title.md
```

**Front-matter format**:

```yaml
---
layout: post
title: 'Post Title'
date: YYYY-MM-DD HH:MM
comments: true
tags:
  - tag1
  - tag2
top: true # For pinned posts (requires hexo-generator-index-pin-top)
---
```

### Markdown Style

- Use `<!--more-->` to define excerpt break
- Code blocks should specify language: ```javascript
- Images: `![alt text](/img/path/to/image.png)`
- External links open in new tab (configured in \_config.yml)

## Code Style

### EJS Templates (themes/yelee/layout/)

- Use 2 spaces for indentation
- Keep template logic minimal; use Hexo helpers
- Comments: `<!-- HTML comment -->` or `<%# EJS comment %>`

### Stylus/CSS (themes/yelee/source/css/)

- Use Stylus preprocessor (indented syntax, no braces)
- 2 spaces indentation
- Group related styles together

### JavaScript

- ESLint configured with Prettier integration
- Run `npm run lint` to check code
- Run `npm run lint:fix` to auto-fix issues
- Follow existing patterns in theme files
- Use semicolons

## Configuration Files

### Site Config (\_config.yml)

Key settings:

- `permalink: :year/:month/:day/:title/`
- `new_post_name: year-:month-:day.:title.md`
- `language: zh-Hans`
- `timezone: Asia/Shanghai`

### Theme Config (themes/yelee/\_config.yml)

- Menu configuration
- Social links (subnav)
- Comment system (Disqus)
- Sidebar widgets

## Dependencies

Core Hexo packages:

- hexo: ^5.0.0
- hexo-generator-index-pin-top: ^0.2.2 (for pinned posts)
- hexo-renderer-ejs: ^1.0.0
- hexo-renderer-marked: ^3.0.0
- hexo-renderer-stylus: ^2.0.0

## Troubleshooting

If `hexo server` fails after cloning:

```bash
rm package-lock.json
rm -rf node_modules
npm install
```

## Linting and Formatting

This project uses:

- **ESLint** for JavaScript linting with Prettier integration
- **Prettier** for code formatting

Configuration files:

- `eslint.config.js` - ESLint configuration (flat config format for ESLint v9+)
- `.prettierrc` - Prettier configuration

When making changes:

1. Run `npm run lint` to check for issues
2. Run `npm run lint:fix` to auto-fix issues
3. Run `npm run format` to format code
4. Verify by running `hexo server` and checking the output

## No Test Setup

This project does not have a test framework.

## Git Workflow

- Build output (`public/`) is not committed
- `node_modules/` is in .gitignore
- Theme files in `themes/yelee/` should be committed
