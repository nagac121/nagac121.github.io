# Naga's Portfolio

A modern, responsive portfolio website showcasing my work as a Senior Full-Stack & Agentic AI Engineer.

## 🌟 Features

- **Home Page**: Professional introduction with call-to-action buttons
- **Projects Section**: Showcase of notable projects including the Market Intelligence Agentic System
- **Blog**: Technical articles on web development and AI engineering
- **Contact Form**: Fully functional form with:
  - Client-side validation (subject length, message length)
  - Honeypot spam protection
  - AJAX submission using Fetch API
  - Handling of email service limits (402/429 status codes)
  - Success/error status messages with visual feedback
- **Responsive Design**: Built with Tailwind CSS for mobile-friendly layouts
- **Static Site Generation**: Powered by Astro for optimal performance

## 🛠️ Tech Stack

- **Framework**: [Astro](https://astro.build) (v6.1.2)
- **Styling**: [Tailwind CSS](https://tailwindcss.com) (v4.2.2) via @tailwindcss/vite
- **Form Handling**: Formspree for email backend
- **Validation**: Zod (v4.3.6) for schema validation
- **TypeScript**: For enhanced developer experience
- **Deployment**: GitHub Pages

## 📁 Project Structure

```
src/
├── components/       # Reusable Astro components (Footer, Navigation)
├── content/          # Markdown content (blog posts)
│   └── blog/
├── layouts/          # Base layout components
├── pages/            # Site pages (index, projects, contact, blog)
└── styles/           # Global CSS styles
```

## 🔧 Installation & Development

1. Clone the repository:

   ```bash
   git clone https://github.com/nagac121/nagac121.github.io.git
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Start the development server:

   ```bash
   npm run dev
   ```

4. Build for production:

   ```bash
   npm run build
   ```

5. Preview the production build:
   ```bash
   npm run preview
   ```

## 📝 Blog Posts

1. **My Journey into Web Development** - Personal journey and learning path
2. **Transitioning to Agentic AI** - Insights on moving into AI engineering

## 📬 Contact Form Features

The contact form includes several sophisticated features:

- **Client-Side Validation**:
  - Subject: 5-100 characters
  - Message: 20-500 characters
  - Real-time feedback without page reload

- **Spam Protection**: Honeypot technique to deter automated bots

- **AJAX Submission**: Uses Fetch API for seamless form submission

- **Error Handling**:
  - Specific handling for email service limits (HTTP 402/429)
  - Generic error fallback for other issues
  - Visual status indicators (success/error states)

- **User Experience**:
  - Loading states during submission
  - Form reset on success
  - Accessible form labels and controls

## 🚀 Deployment

This site is configured to deploy automatically to GitHub Pages via the `.github.io` domain. The `build` script generates static files in the `dist/` directory which are served by GitHub Pages.

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 👤 Author

**Naga** - Senior Full-Stack & Agentic AI Engineer

- [View Projects](/projects)
- [Read Blog](/blog)
- [Contact Me](/contact)

---

_Built with Astro, Tailwind CSS, and a passion for creating elegant, functional web experiences._
