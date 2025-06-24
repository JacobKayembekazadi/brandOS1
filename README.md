# Brand Central AI 🚀

> **AI-Powered Brand Development Platform** - Transform your brand strategy with intelligent automation and data-driven insights

[![React](https://img.shields.io/badge/React-19.1.0-blue?logo=react)](https://reactjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.7.2-blue?logo=typescript)](https://typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-6.2.0-purple?logo=vite)](https://vitejs.dev/)
[![Google Gemini](https://img.shields.io/badge/AI-Google%20Gemini-orange?logo=google)](https://ai.google.dev/)

## 🎯 Overview

Brand Central AI is a comprehensive brand development platform that leverages Google Gemini AI to guide businesses through a structured 10-phase brand creation journey. From audience profiling to content generation, it provides intelligent tools for building cohesive, data-driven brand strategies.

### ✨ Key Features

- **🧠 AI-Powered Content Generation** - Gemini-driven content ideas and brand validation
- **📊 10-Phase Brand Journey** - Structured progression from foundation to distribution
- **🎨 Visual Identity Kit** - Colors, fonts, and logo management system
- **🌍 World Bible Creation** - Comprehensive brand lore and narrative development
- **📈 Performance Analytics** - North Star metrics and L-M-A-I scoring system
- **🔄 Real-time Collaboration** - Activity tracking and community engagement tools

## 🏗️ Architecture

Built with modern web technologies for performance, scalability, and developer experience:

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   React 19      │    │  Google Gemini   │    │  localStorage   │
│   TypeScript    │◄──►│  AI Integration  │◄──►│  Data Layer     │
│   Vite Build    │    │  API Service     │    │  JSON Persist   │
└─────────────────┘    └──────────────────┘    └─────────────────┘
         ▲                        ▲                        ▲
         │                        │                        │
┌─────────────────────────────────────────────────────────────────┐
│                    Core Application Modules                     │
├─────────────────┬─────────────────┬─────────────────┬──────────┤
│    Dashboard    │  World Foundry  │ Content Studio  │Distribution│
│   • Metrics     │  • Positioning  │ • AI Generation │  • Analytics│
│   • Navigation  │  • Audience     │ • Brand Guard   │  • Tracking │
│   • Activity    │  • Identity     │ • Templates     │  • Insights │
└─────────────────┴─────────────────┴─────────────────┴──────────┘
```

## 🚀 Quick Start

### Prerequisites

- **Node.js** 18+ ([Download](https://nodejs.org/))
- **Google Gemini API Key** ([Get yours](https://ai.google.dev/))

### Installation

```bash
# Clone the repository
git clone https://github.com/JacobKayembekazadi/brandOS1.git
cd brandOS1

# Install dependencies
npm install

# Set up environment variables
cp process.env.API_KEY.env .env.local
# Edit .env.local and add your GEMINI_API_KEY

# Start development server
npm run dev
```

🌐 Open [http://localhost:5173](http://localhost:5173) to view the application.

### 🔑 API Key Setup

1. Visit [Google AI Studio](https://ai.google.dev/)
2. Create a new API key for Gemini
3. Add it to your `.env.local` file:
   ```
   GEMINI_API_KEY=your_api_key_here
   ```

## 📋 Available Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server with hot reload |
| `npm run build` | Build production-ready application |
| `npm run preview` | Preview production build locally |
| `npm run lint` | Run TypeScript type checking |

## 🏢 Application Modules

### 📊 Dashboard
- **North Star Metrics** - Track key brand performance indicators
- **Phase Navigator** - Visual progress through 10-phase journey
- **Recent Activity** - Real-time updates and action history
- **L-M-A-I Check** - Brand alignment scoring system

### 🌍 World Foundry
- **Audience Architect** - AI-powered customer persona development
- **Macro Fantasy Generator** - Brand positioning statement creation
- **Visual Identity Kit** - Color palettes, typography, and logo assets
- **Competitive Analysis** - Market positioning and differentiation

### 🎨 Content Studio
- **AI Idea Generator** - Context-aware content brainstorming
- **Brand Guardian** - Content validation against brand guidelines
- **Template Engine** - Structured content creation workflows
- **Prompt Engineering** - Advanced AI prompt optimization

### 📈 Distribution Hub
- **Community Engagement** - Social media response suggestions
- **Performance Analytics** - Content performance tracking
- **Publishing Tools** - Multi-platform content distribution

## 🗂️ Project Structure

```
brand-os-app1/
├── 📁 components/          # React components organized by module
│   ├── 📁 dashboard/       # Dashboard module components
│   ├── 📁 worldfoundry/    # Brand foundation tools
│   ├── 📁 contentstudio/   # AI content creation tools
│   ├── 📁 distribution/    # Publishing and analytics
│   └── 📁 shared/          # Reusable UI components
├── 📁 services/            # API and business logic
├── 📁 types.ts            # TypeScript type definitions
├── 📁 constants.ts        # Application constants and configuration
├── 📁 ADR/                # Architecture Decision Records
├── 📄 architectural_document.md    # Technical architecture guide
├── 📄 product_requirements_document.md  # Product specifications
└── 📄 README.md           # This file
```

## 🔧 Configuration

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `GEMINI_API_KEY` | Google Gemini API key for AI features | Yes |
| `NODE_ENV` | Environment (development/production) | Auto-set |

### Browser Support

- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Edge 90+

## 📚 Documentation

- **[Architecture Guide](./architectural_document.md)** - Technical architecture and design decisions
- **[Product Requirements](./product_requirements_document.md)** - Detailed feature specifications
- **[ADR Directory](./ADR/)** - Architecture Decision Records for key technical choices

## 🚀 Deployment

### Static Hosting (Recommended)

Deploy to any static hosting platform:

```bash
# Build for production
npm run build

# Deploy to your preferred platform:
# - Vercel: vercel --prod
# - Netlify: netlify deploy --prod
# - GitHub Pages: via GitHub Actions
```

### Supported Platforms

- **Vercel** ⭐ (Recommended)
- **Netlify**
- **GitHub Pages**
- **AWS S3 + CloudFront**

## 🤝 Contributing

We welcome contributions! Please see our [Architecture Decision Records](./ADR/) for technical context.

### Development Workflow

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feature/amazing-feature`
3. **Commit** your changes: `git commit -m 'Add amazing feature'`
4. **Push** to the branch: `git push origin feature/amazing-feature`
5. **Open** a Pull Request

### Code Standards

- **TypeScript** for type safety
- **ESLint** for code quality
- **Prettier** for code formatting
- **Component-based** architecture

## 📊 Performance

- ⚡ **First Load**: < 3 seconds
- 🔄 **Navigation**: < 500ms
- 📱 **Mobile Optimized**: Responsive design
- 🌐 **Offline Ready**: localStorage persistence

## 🛠️ Technology Stack

| Category | Technology | Version | Purpose |
|----------|------------|---------|---------|
| **Frontend** | React | 19.1.0 | UI framework |
| **Language** | TypeScript | 5.7.2 | Type safety |
| **Build Tool** | Vite | 6.2.0 | Development & bundling |
| **AI Platform** | Google Gemini | API | Content generation |
| **Styling** | CSS3 | - | Component styling |
| **Routing** | React Router | 6.28.0 | Client-side routing |
| **Storage** | localStorage | - | Data persistence |

## 📈 Roadmap

### 🎯 Current (v1.0)
- ✅ Core brand development workflow
- ✅ AI-powered content generation
- ✅ Local data persistence
- ✅ Responsive design

### 🔮 Future (v2.0+)
- 🔄 Cloud synchronization
- 👥 Team collaboration features
- 📊 Advanced analytics dashboard
- 🎨 Extended visual identity tools
- 🔌 Third-party integrations

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/JacobKayembekazadi/brandOS1/issues)
- **Discussions**: [GitHub Discussions](https://github.com/JacobKayembekazadi/brandOS1/discussions)
- **Documentation**: [Architecture Guide](./architectural_document.md)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Google Gemini** - AI content generation capabilities
- **React Team** - Frontend framework excellence
- **Vite Team** - Lightning-fast development experience
- **Open Source Community** - Inspiration and tools

---

<div align="center">

**Built with ❤️ for modern brand builders**

[🌟 Star this repo](https://github.com/JacobKayembekazadi/brandOS1) • [🐛 Report Bug](https://github.com/JacobKayembekazadi/brandOS1/issues) • [💡 Request Feature](https://github.com/JacobKayembekazadi/brandOS1/issues)

</div>
