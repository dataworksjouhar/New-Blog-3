# Mohammed Jouhar's Personal Blog Website

A clean, professional, and mobile-friendly personal blog website built with Jekyll. This blog is designed to share practical insights in Data Analytics, AI Agents, and career development based on real-world experience.

## Features

- **Clean, Minimalist Design**: Professional layout with responsive design for all devices
- **Dark Mode Support**: Toggle between light and dark themes
- **Category System**: Content organized by categories for easy navigation
- **Tagging System**: Additional content organization with tags
- **SEO Optimized**: Meta tags, structured data, and sitemap included
- **Mobile-Friendly**: Fully responsive design works on all screen sizes
- **Code Syntax Highlighting**: Beautiful code formatting for technical posts
- **Social Sharing**: Easy sharing of posts to social media
- **RSS Feed**: Subscribe to updates via RSS
- **Search Functionality**: Find content quickly with built-in search

## Getting Started

### Prerequisites

- Ruby version 2.5.0 or higher
- RubyGems
- GCC and Make

### Installation

1. **Install Jekyll and Bundler gems**

```bash
gem install jekyll bundler
```

2. **Clone this repository**

```bash
git clone https://github.com/yourusername/my-dataworks-blog.git
cd my-dataworks-blog
```

3. **Install dependencies**

```bash
bundle install
```

4. **Run the development server**

```bash
bundle exec jekyll serve
```

5. **View your site**

Open your browser and go to `http://localhost:4000`

## Customization

### Site Configuration

The main configuration file is `_config.yml`. Edit this file to customize:

- Site title and description
- Author information
- Social media links
- Default front matter settings
- Categories and tags

### Content Management

#### Blog Posts

All blog posts are stored in the `_posts` directory. Create new posts by adding Markdown files with the following naming convention:

```
YYYY-MM-DD-title-of-post.md
```

Each post should include front matter at the top:

```yaml
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD HH:MM:SS +0000
categories: [Category Name]
tags: [tag1, tag2, tag3]
---
```

#### Pages

Static pages are stored in the root directory. Edit the existing pages or create new ones with front matter:

```yaml
---
layout: page
title: "Page Title"
permalink: /page-url/
---
```

### Design Customization

#### CSS Styling

The main CSS file is located at `assets/css/main.css`. Edit this file to customize the site's appearance.

#### Layouts

Layout templates are stored in the `_layouts` directory:

- `default.html`: The base layout for all pages
- `page.html`: Layout for static pages
- `post.html`: Layout for blog posts
- `category.html`: Layout for category pages
- `tag.html`: Layout for tag pages

#### Includes

Reusable components are stored in the `_includes` directory. Customize these to modify specific parts of the site.

## Deployment

### GitHub Pages

This site is designed to be easily deployed to GitHub Pages:

1. Create a GitHub repository named `yourusername.github.io`
2. Push your site to the repository:

```bash
git remote add origin https://github.com/yourusername/yourusername.github.io.git
git push -u origin main
```

3. Your site will be available at `https://yourusername.github.io`

### Other Hosting Options

The site can also be deployed to any hosting service that supports static websites:

1. Build the site:

```bash
bundle exec jekyll build
```

2. The complete site will be generated in the `_site` directory
3. Upload the contents of the `_site` directory to your hosting provider

## Adding New Content

### Creating New Posts

1. Create a new Markdown file in the `_posts` directory with the naming convention `YYYY-MM-DD-title.md`
2. Add front matter with title, date, categories, and tags
3. Write your content using Markdown
4. Save the file and rebuild the site

### Adding Images

1. Place images in the `assets/images` directory
2. Reference them in your posts using Markdown:

```markdown
![Image description](/assets/images/your-image.jpg)
```

### Code Blocks

Use triple backticks with the language name for syntax highlighting:

````markdown
```python
def hello_world():
    print("Hello, world!")
```
````

### Tables

Create tables using Markdown syntax:

```markdown
| Header 1 | Header 2 |
|----------|----------|
| Cell 1   | Cell 2   |
| Cell 3   | Cell 4   |
```

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- Built with [Jekyll](https://jekyllrb.com/)
- Theme inspired by modern design principles
- Sample content created to demonstrate the site's capabilities

## Support

For questions or support, please contact Mohammed Jouhar via the contact page or LinkedIn.
