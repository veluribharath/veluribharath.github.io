# CLAUDE.md - AI Assistant Guide

**Last Updated:** 2025-11-19
**Repository:** veluribharath.github.io
**Project Type:** Jekyll-based GitHub Pages Resume/Portfolio Website
**Owner:** Bharath Veluri

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Codebase Structure](#codebase-structure)
3. [Development Workflows](#development-workflows)
4. [Key Conventions](#key-conventions)
5. [Common Tasks](#common-tasks)
6. [File Reference](#file-reference)
7. [Technology Stack](#technology-stack)
8. [Design Patterns](#design-patterns)
9. [Git Guidelines](#git-guidelines)

---

## Project Overview

This is a **Jekyll-based personal resume/portfolio website** hosted on GitHub Pages. The site showcases professional information including career profile, education, work experience, technical skills, projects, and contact information.

### Key Characteristics
- **Data-Driven Architecture:** All content managed through `_data/data.yml`
- **Theme System:** 6 color themes controlled via `_config.yml`
- **Responsive Design:** Mobile-first with Bootstrap 3
- **Component-Based Templates:** Modular includes for each section
- **Static Site Generation:** Jekyll compiles to static HTML/CSS/JS

### Site Information
- **Live URL:** http://veluribharath.github.io
- **Current Theme:** Blue (configurable)
- **License:** MIT (webjeda, 2019)
- **Template:** Orbit by Xiaoying Riley (3rdwavemedia)

---

## Codebase Structure

```
veluribharath.github.io/
├── _config.yml                 # Jekyll site configuration
├── _data/
│   └── data.yml               # ALL resume content lives here
├── _layouts/
│   ├── default.html           # Main page layout structure
│   └── compress.html          # HTML compression wrapper
├── _includes/                 # Reusable template components (13 files)
│   ├── head.html             # <head> metadata and CSS
│   ├── sidebar.html          # Left sidebar with profile
│   ├── footer.html           # Site footer
│   ├── scripts.html          # JavaScript includes
│   ├── career-profile.html   # Career summary section
│   ├── experiences.html      # Work experience timeline
│   ├── education.html        # Education history
│   ├── skills.html           # Technical skills with progress bars
│   ├── projects.html         # Portfolio projects
│   ├── publications.html     # Publications list
│   ├── contact.html          # Contact form
│   ├── language.html         # Languages spoken
│   ├── interests.html        # Personal interests
│   ├── about.html            # About me section
│   └── analytics.html        # Google Analytics integration
├── _sass/                     # SCSS source files (576 lines total)
│   ├── _base.scss            # Base styles (274 lines)
│   ├── _default.scss         # Main import orchestrator
│   ├── _mixins.scss          # CSS mixins (186 lines, 24+ mixins)
│   ├── _responsive.scss      # Media queries (65 lines)
│   ├── _print.scss           # Print stylesheet
│   └── skins/                # Theme color variants (6 themes)
│       ├── _blue.scss        # Default theme
│       ├── _orange.scss
│       ├── _green.scss
│       ├── _turquoise.scss
│       ├── _berry.scss
│       └── _ceramic.scss
├── assets/
│   ├── css/
│   │   └── main.scss         # SCSS entry point (Jekyll compiles this)
│   ├── js/
│   │   ├── main.js           # Custom JavaScript (skill bar animations)
│   │   └── min/              # Minified JS variants
│   ├── images/               # Profile pictures and assets
│   │   ├── profile.jpeg
│   │   ├── profile.png
│   │   ├── profile_small.png # 100x100px for sidebar
│   │   └── orange.jpg
│   └── plugins/              # Third-party libraries (~15MB)
│       ├── bootstrap/        # Bootstrap 3 (1.2MB)
│       ├── font-awesome/     # Font Awesome 5 (12MB)
│       └── jquery-1.11.3.min.js (94KB)
├── index.html                # Main page entry point
├── favicon.ico
├── README.md
├── LICENSE.txt
└── CLAUDE.md                 # This file
```

---

## Development Workflows

### Making Content Changes

**CRITICAL:** Content changes should ONLY be made to `_data/data.yml`. Do NOT edit individual template files unless modifying the structure itself.

#### Step 1: Edit Content in `_data/data.yml`

```yaml
# Example: Update job title
sidebar:
  tagline: Data Scientist  # Change this line

# Example: Add new skill
skills:
  - name: Python
    level: 98%
  - name: NewTech  # Add new skill here
    level: 85%
```

#### Step 2: Test Locally (if applicable)

```bash
# If Jekyll is installed locally:
bundle exec jekyll serve
# Site available at http://localhost:4000
```

#### Step 3: Commit and Push

```bash
git add _data/data.yml
git commit -m "Update [specific change description]"
git push -u origin [branch-name]
```

### Making Style Changes

#### Changing Theme Color

1. Edit `_config.yml`:
   ```yaml
   theme_skin: blue  # Options: blue, turquoise, green, berry, orange, ceramic
   ```

2. Rebuild site (auto on GitHub Pages)

#### Modifying Styles

1. **For theme colors:** Edit `_sass/skins/_[theme].scss`
2. **For layout/structure:** Edit `_sass/_base.scss`
3. **For responsive behavior:** Edit `_sass/_responsive.scss`
4. **For mixins/utilities:** Edit `_sass/_mixins.scss`

### Adding New Sections

#### Step 1: Create Include Template

Create `_includes/new-section.html`:

```liquid
{% assign section = site.data.data.newsection %}
{% if section %}
<section class="section newsection-section">
  <h2 class="section-title">
    <span class="fa-stack fa-xs">
      <i class="fas fa-circle fa-stack-2x"></i>
      <i class="fas fa-icon-name fa-stack-1x fa-inverse"></i>
    </span>
    {{ section.title }}
  </h2>

  {% for item in section.items %}
    <div class="item">
      <h3>{{ item.name }}</h3>
      <p>{{ item.description }}</p>
    </div>
  {% endfor %}
</section>
{% endif %}
```

#### Step 2: Add Data to `_data/data.yml`

```yaml
newsection:
  title: "New Section Title"
  items:
    - name: "Item 1"
      description: "Description here"
```

#### Step 3: Include in Layout

Edit `_layouts/default.html`:

```liquid
{% include new-section.html %}
```

---

## Key Conventions

### Data-Driven Content

**GOLDEN RULE:** All content lives in `_data/data.yml`. Templates are structural only.

```yaml
# _data/data.yml structure:
sidebar:          # Left sidebar content
  name: ""
  tagline: ""
  avatar: ""
  email: ""
  phone: ""
  website: ""
  linkedin: ""
  github: ""
  languages: []
  interests: []

career-profile:   # Career summary
  title: ""
  summary: ""

education: []     # Education history
experiences: []   # Work experience
projects: []      # Portfolio projects
publications: []  # Publications
skills: []        # Technical skills
```

### Theme System

Themes are defined in `_sass/skins/` with these SCSS variables:

```scss
$theme-color: #42A8C0;           // Primary brand color
$text-color: #3F4650;            // Main text
$text-color-secondary: #545E6C;  // Secondary text
$text-grey: #97AAC3;             // Muted text
$divider: #e8e8e8;               // Borders/dividers
$lighter-grey: #ccc;
$darker-grey: #666;
$smoky-white: #f5f5f5;           // Backgrounds
$heart: #fb866a;                 // Accent color
```

### Responsive Breakpoints

```scss
// Extra small (phones): < 767px
@media (max-width: 767px) { /* Phone styles */ }

// Small (tablets): 768px+
@media (min-width: 768px) { /* Tablet styles */ }

// Medium (desktops): 992px+
@media (min-width: 992px) { /* Desktop styles */ }

// Large (large desktops): 1200px+
@media (min-width: 1200px) { /* Large desktop styles */ }

// Extra-large: 1400px+
@media (min-width: 1400px) { /* XL desktop styles */ }
```

### SCSS Mixin Library

24+ mixins available in `_sass/_mixins.scss`:

```scss
// Shadows
@include text-shadow($shadow);
@include box-shadow($shadow);
@include drop-shadow($shadow);

// Transforms
@include scale($factor);
@include rotate($deg);
@include translate($x, $y);
@include translate3d($x, $y, $z);

// Transitions & Animations
@include transition($args);
@include animation($args);

// Borders & Shapes
@include border-radius($radius);
@include border-top-radius($radius);
@include border-bottom-radius($radius);

// Backgrounds
@include vertical-gradient($from, $to);
@include horizontal-gradient($from, $to);
@include bg-size($size);

// And more...
```

### HTML Component Pattern

Each section include follows this pattern:

```liquid
{% assign section = site.data.data.sectionname %}
{% if section %}
<section class="section sectionname-section">
  <h2 class="section-title">
    <span class="fa-stack fa-xs">
      <i class="fas fa-circle fa-stack-2x"></i>
      <i class="fas fa-icon fa-stack-1x fa-inverse"></i>
    </span>
    {{ section.title }}
  </h2>

  <!-- Section content here -->
  {{ section.content | markdownify }}

</section>
{% endif %}
```

### File Naming Conventions

- **Layouts:** `_layouts/[name].html`
- **Includes:** `_includes/[section-name].html`
- **SCSS Partials:** `_sass/_[name].scss`
- **Skins:** `_sass/skins/_[color].scss`
- **Data:** `_data/data.yml` (single file)
- **Assets:** `assets/[type]/[name].[ext]`

---

## Common Tasks

### 1. Update Profile Information

**File:** `_data/data.yml`

```yaml
sidebar:
  name: Bharath Veluri
  tagline: Data Scientist
  avatar: profile_small.png
  email: your.email@domain.com
  phone: 012 345 6789
  website: veluribharath.github.io
  linkedin: username
  github: username
```

### 2. Add Work Experience

**File:** `_data/data.yml`

```yaml
experiences:
  - role: Senior Developer
    time: 2020 - Present
    company: Company Name, Location
    details: |
      Describe your responsibilities and achievements.
      - Bullet point 1
      - Bullet point 2

      You can use **markdown** here.
```

### 3. Update Skills

**File:** `_data/data.yml`

```yaml
skills:
  title: Skills &amp; Proficiency

  toolset:
    - name: Python
      level: 98%

    - name: JavaScript
      level: 85%

    # Percentage controls width of progress bar
```

### 4. Change Profile Picture

1. Add new image to `assets/images/`
2. Update `_data/data.yml`:
   ```yaml
   sidebar:
     avatar: your-new-image.png
   ```
3. Recommended size: 100x100px for sidebar

### 5. Switch Color Theme

**File:** `_config.yml`

```yaml
theme_skin: turquoise  # blue, turquoise, green, berry, orange, ceramic
```

### 6. Add Custom SCSS

**File:** `_sass/_base.scss` (or create new partial and import)

```scss
// Add at end of _base.scss
.custom-class {
  @include box-shadow(0 2px 4px rgba(0,0,0,.1));
  border-radius: 4px;
}
```

### 7. Modify Layout Structure

**File:** `_layouts/default.html`

- Two-column layout: `.sidebar-wrapper` (280px) + `.main-wrapper`
- Edit carefully as structure affects responsive behavior

### 8. Update Contact Links

**File:** `_data/data.yml`

Social links auto-generate URLs:
```yaml
sidebar:
  linkedin: your-username    # → https://linkedin.com/in/your-username
  github: your-username      # → http://github.com/your-username
  twitter: '@yourhandle'     # → https://twitter.com/yourhandle
```

---

## File Reference

### Critical Files (Modify Frequently)

| File | Purpose | Edit When |
|------|---------|-----------|
| `_data/data.yml` | All resume content | Updating any content |
| `_config.yml` | Site configuration | Changing theme, title, URL |
| `assets/images/profile_small.png` | Sidebar photo | Updating profile picture |

### Template Files (Modify Occasionally)

| File | Purpose | Lines | Edit When |
|------|---------|-------|-----------|
| `_layouts/default.html` | Main layout structure | - | Changing layout structure |
| `_includes/sidebar.html` | Sidebar template | - | Modifying sidebar layout |
| `_includes/experiences.html` | Experience section | - | Changing experience display |
| `_includes/skills.html` | Skills section | - | Modifying skill bars |
| `_includes/education.html` | Education section | - | Changing education layout |

### Style Files (Modify for Design Changes)

| File | Purpose | Lines | Edit When |
|------|---------|-------|-----------|
| `_sass/_base.scss` | Core styles | 274 | Changing base styles |
| `_sass/_mixins.scss` | SCSS mixins | 186 | Adding/modifying mixins |
| `_sass/_responsive.scss` | Media queries | 65 | Adjusting responsive behavior |
| `_sass/skins/_blue.scss` | Blue theme colors | 10 | Customizing blue theme |
| `assets/css/main.scss` | SCSS entry point | - | Changing import order |

### Configuration Files (Rarely Modify)

| File | Purpose | Edit When |
|------|---------|-----------|
| `_config.yml` | Jekyll config | Changing build settings |
| `index.html` | Entry point | Changing front matter |
| `favicon.ico` | Browser icon | Updating favicon |

---

## Technology Stack

### Core Technologies

| Technology | Version | Purpose | Documentation |
|------------|---------|---------|---------------|
| **Jekyll** | Latest | Static site generator | https://jekyllrb.com |
| **Liquid** | - | Template language | https://shopify.github.io/liquid/ |
| **SCSS/Sass** | - | CSS preprocessing | https://sass-lang.com |
| **Bootstrap** | 3.x | CSS framework | https://getbootstrap.com/docs/3.4/ |
| **Font Awesome** | 5.x | Icon library | https://fontawesome.com |
| **jQuery** | 1.11.3 | JavaScript library | https://jquery.com |

### Build Process

```
1. Jekyll reads _config.yml and _data/data.yml
2. Processes Liquid templates in _layouts/ and _includes/
3. Compiles SCSS (assets/css/main.scss → main.css)
4. Copies static assets (images, plugins, favicon)
5. Generates static HTML in _site/ (on local builds)
6. GitHub Pages hosts the compiled site
```

### Browser Support

- Modern browsers (Chrome, Firefox, Safari, Edge)
- IE8/IE9 support via HTML5 shiv and Respond.js
- Mobile browsers (iOS Safari, Chrome Mobile)

---

## Design Patterns

### 1. Data-Driven Architecture

**Pattern:** Separate content from presentation

**Implementation:**
- Content: `_data/data.yml`
- Structure: `_layouts/` and `_includes/`
- Style: `_sass/`

**Benefits:**
- Easy content updates without touching code
- Non-technical users can edit YAML
- Consistent structure across sections

### 2. Component-Based Templates

**Pattern:** Modular, reusable includes

**Implementation:**
```liquid
<!-- _layouts/default.html -->
{% include sidebar.html %}
{% include career-profile.html %}
{% include experiences.html %}
```

**Benefits:**
- Single responsibility per component
- Easy to add/remove sections
- Conditional rendering with `{% if %}`

### 3. Theme System

**Pattern:** SCSS variable-based theming

**Implementation:**
```scss
// _sass/skins/_blue.scss
$theme-color: #42A8C0;

// _sass/_base.scss uses:
color: $theme-color;
```

**Benefits:**
- Quick theme switching
- Consistent color usage
- Easy to create new themes

### 4. Progressive Enhancement

**Pattern:** Base functionality first, enhancements layered

**Implementation:**
- HTML structure works without CSS/JS
- CSS adds styling
- JavaScript adds animations
- Responsive CSS adapts to screen size

### 5. Mobile-First Responsive

**Pattern:** Design for mobile, enhance for desktop

**Implementation:**
```scss
// Base styles for mobile
.element { width: 100%; }

// Enhance for larger screens
@media (min-width: 768px) {
  .element { width: 50%; }
}
```

---

## Git Guidelines

### Branch Strategy

**Main Branch:** Not specified (likely `master` or `main`)

**Feature Branches:** Use descriptive names
```bash
# Current feature branch example:
claude/claude-md-mi6mzaazh91muhu5-01VXosBGk35MqC7ebLmVAnkF
```

### Commit Message Conventions

Based on recent commits, use clear, concise messages:

```bash
# Good examples:
git commit -m "removed tracker"
git commit -m "add interests"
git commit -m "Changed Designation"
git commit -m "update data and photo"
git commit -m "update url's"

# Pattern: lowercase verb + specific change
```

### Common Git Commands

```bash
# Check status
git status

# Add files
git add _data/data.yml

# Commit changes
git commit -m "update work experience"

# Push to remote (feature branch)
git push -u origin claude/claude-md-mi6mzaazh91muhu5-01VXosBGk35MqC7ebLmVAnkF

# Fetch updates
git fetch origin

# Pull latest changes
git pull origin [branch-name]
```

### Files to Commit

**Always commit:**
- `_data/data.yml` (content changes)
- `_config.yml` (configuration changes)
- New images in `assets/images/`
- SCSS changes in `_sass/`
- Template changes in `_includes/` or `_layouts/`

**Never commit:**
- `_site/` (Jekyll build output - auto-generated)
- `.DS_Store` (already in repo, but ignore new ones)
- Local test files

### GitHub Pages Deployment

**Automatic:** Every push to the main branch triggers a GitHub Pages rebuild.

**Manual Check:**
1. Go to repository Settings → Pages
2. Check build status
3. Visit live site: http://veluribharath.github.io

---

## AI Assistant Guidelines

### When Making Changes

1. **Always read files before editing** - Use Read tool before Edit/Write
2. **Preserve exact formatting** - Match indentation (spaces vs tabs)
3. **Validate YAML syntax** - Use proper indentation (2 spaces)
4. **Test data structure** - Ensure YAML structure matches template expectations
5. **Check for consistency** - Match existing patterns and conventions

### Content Updates

**DO:**
- Edit `_data/data.yml` for all content changes
- Use markdown in YAML for formatted text
- Follow existing YAML structure
- Validate YAML syntax before committing

**DON'T:**
- Edit individual include files for content
- Mix tabs and spaces in YAML
- Remove required YAML keys
- Break existing data structure

### Style Changes

**DO:**
- Use existing SCSS mixins from `_mixins.scss`
- Follow mobile-first responsive approach
- Test across breakpoints
- Maintain consistent spacing (2 spaces)

**DON'T:**
- Add vendor prefixes manually (use mixins)
- Break existing responsive behavior
- Modify plugin files (Bootstrap, Font Awesome)
- Add new dependencies without discussion

### Template Changes

**DO:**
- Follow Liquid syntax conventions
- Use conditional rendering (`{% if %}`)
- Apply `markdownify` filter for markdown content
- Maintain semantic HTML structure

**DON'T:**
- Hardcode content in templates
- Break existing includes
- Remove conditional checks
- Change layout structure without understanding impact

### Common Pitfalls to Avoid

1. **YAML Indentation Errors**
   ```yaml
   # Wrong:
   experiences:
   - role: Developer  # Needs indentation

   # Correct:
   experiences:
     - role: Developer
   ```

2. **Forgetting Liquid Filters**
   ```liquid
   <!-- Wrong: -->
   {{ section.summary }}

   <!-- Correct (for markdown): -->
   {{ section.summary | markdownify }}
   ```

3. **Breaking Responsive Layout**
   ```scss
   // Don't override without understanding:
   .sidebar-wrapper { width: 300px; }  // Breaks mobile layout
   ```

4. **Mixing Content and Structure**
   ```liquid
   <!-- Wrong (hardcoded content): -->
   <h1>Bharath Veluri</h1>

   <!-- Correct (data-driven): -->
   <h1>{{ site.data.data.sidebar.name }}</h1>
   ```

---

## Quick Reference

### Essential File Paths

```
Content:    _data/data.yml
Config:     _config.yml
Main CSS:   assets/css/main.scss
Main JS:    assets/js/main.js
Layout:     _layouts/default.html
Profile:    assets/images/profile_small.png
```

### Essential Commands

```bash
# Local development (if Jekyll installed)
bundle exec jekyll serve

# Check git status
git status

# Commit content change
git add _data/data.yml && git commit -m "update [what changed]"

# Push to feature branch
git push -u origin [branch-name]
```

### Essential Variables (in templates)

```liquid
{{ site.data.data.sidebar.name }}       # Profile name
{{ site.data.data.sidebar.tagline }}    # Job title
{{ site.data.data.sidebar.email }}      # Email
{{ site.title }}                         # Site title from config
{{ site.theme_skin }}                    # Current theme
```

### Essential SCSS Variables (in themes)

```scss
$theme-color           // Primary brand color
$text-color            // Main text color
$text-color-secondary  // Secondary text
$smoky-white           // Background color
$divider               // Border color
```

---

## Troubleshooting

### Common Issues

**Issue:** YAML parsing error
**Solution:** Check indentation (2 spaces, no tabs), validate at yamllint.com

**Issue:** Changes not showing on live site
**Solution:** Wait 1-2 minutes for GitHub Pages rebuild, check commit was pushed

**Issue:** Styles not applying
**Solution:** Check SCSS syntax, ensure file imported in main.scss

**Issue:** Section not showing
**Solution:** Verify data exists in `_data/data.yml`, check include is in layout

**Issue:** Responsive layout broken
**Solution:** Check media queries, ensure proper mobile-first approach

### Validation Checklist

Before committing changes:

- [ ] YAML syntax is valid (use yamllint or YAML parser)
- [ ] SCSS compiles without errors
- [ ] Changes follow existing patterns
- [ ] No hardcoded content in templates
- [ ] Responsive behavior maintained
- [ ] Commit message is descriptive
- [ ] Only necessary files are staged

---

## Additional Resources

### Official Documentation
- Jekyll: https://jekyllrb.com/docs/
- Liquid: https://shopify.github.io/liquid/
- Bootstrap 3: https://getbootstrap.com/docs/3.4/
- Font Awesome: https://fontawesome.com/icons
- SCSS: https://sass-lang.com/documentation

### Template Resources
- Original Orbit template: http://themes.3rdwavemedia.com/website-templates/orbit-free-resume-cv-template-for-developers/
- GitHub Pages: https://pages.github.com

### Tools
- YAML Validator: http://www.yamllint.com
- SCSS Compiler: https://www.sassmeister.com
- Markdown Preview: https://dillinger.io

---

## Version History

| Date | Version | Changes |
|------|---------|---------|
| 2025-11-19 | 1.0 | Initial CLAUDE.md creation with comprehensive documentation |

---

## Contact

For questions about this repository:
- **Repository Owner:** Bharath Veluri
- **GitHub:** veluribharath
- **Repository:** https://github.com/veluribharath/veluribharath.github.io

---

**Note for AI Assistants:** This document is designed to help you understand and work with this codebase effectively. Always prioritize the conventions and patterns documented here to maintain consistency and code quality. When in doubt, examine existing files for patterns before making changes.
