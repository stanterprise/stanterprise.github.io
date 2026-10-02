# Observer Documentation Site

This repository contains the documentation website for Observer, a comprehensive developer tool for modern observability.

## Built With

- [Hugo](https://gohugo.io/) - Static site generator
- [Docsy](https://www.docsy.dev/) - Hugo theme for technical documentation
- [Mermaid](https://mermaid.js.org/) - Diagram and flowchart generation

## Local Development

### Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) v0.156.0 or later (Extended version required for SCSS support)
- [Go](https://golang.org/dl/) v1.21 or later
- [Node.js](https://nodejs.org/) v24 or later

> **Note**: The GitHub Actions workflow uses Hugo v0.156.0 and Node.js v24. For consistency, use these versions for local development.

### Setup

1. Clone the repository:

   ```bash
   git clone https://github.com/stanterprise/stanterprise.github.io.git
   cd stanterprise.github.io
   ```

2. Install dependencies:

   ```bash
   npm install
   hugo mod get
   ```

3. Run the development server:

   ```bash
   hugo server
   ```

4. Open your browser to http://localhost:1313

## Building

To build the site for production:

```bash
hugo --gc --minify
```

The generated site will be in the `public/` directory.

## Deployment

The site is automatically deployed to GitHub Pages when changes are pushed to the `master` or `main` branch via GitHub Actions.

## Content Structure

- `content/_index.md` - Home page
- `content/docs/` - Documentation sections
  - `getting-started/` - Quick start guide
  - `install/` - Installation instructions
  - `architecture/` - Architecture overview
  - `integrations/` - Integration guides
  - `demo/` - Demo and sample applications
- `content/community/` - Community resources

## Contributing

We welcome contributions! Please see our contributing guidelines for details.

## License

Copyright © 2024 Observer. All rights reserved.
