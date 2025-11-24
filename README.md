<img width="1184" height="576" alt="MagicAi - Fitness Life Mantra AI Assistant" loading="lazy" src="https://github.com/user-attachments/assets/d6ba80ff-a62a-4920-b266-85c4a89d6076" />

<div align="center">

# MagicAi (Fitness Life Mantra)

**Your AI-Powered Fitness & Wellness Coach**

[![MCP Supported](https://img.shields.io/badge/MCP-Supported-00c853)](https://modelcontextprotocol.io/introduction)
[![Local First](https://img.shields.io/badge/Local-First-blue)](https://localfirstweb.dev/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Built on Neocortex](https://img.shields.io/badge/Built%20on-Neocortex-purple)](https://github.com/ankityadavv2014/neocortex)

[Documentation](#-guides) • [Setup Guide](./docs/FLM-SETUP.md) • [Contributing](./CONTRIBUTING.md)

</div>

---

## About MagicAi

**MagicAi** is a specialized AI assistant for **Fitness Life Mantra**, helping you live life to the fullest—physically, mentally, and emotionally—through personalized guidance, movement routines, and daily rituals.

Built on the powerful [Neocortex](https://github.com/ankityadavv2014/neocortex) open-source framework, MagicAi combines:

• **Local-First AI** - Run entirely offline with Ollama/vLLM for complete privacy  
• **Multi-AI Support** - Integrates OpenAI, Anthropic, Google, xAI, Ollama, and more  
• **Powerful Tools** - MCP protocol, web search, code execution, data visualization  
• **Custom Agents** - Create specialized fitness coaches with unique personas  
• **Voice Assistant** - Real-time voice chat with full tool integration  
• **Embeddable** - Can be integrated into websites, apps, and platforms via API or iframe

## Why MagicAi?

MagicAi is designed as an **embeddable AI assistant** that can be integrated into:
- The Fitness Life Mantra website
- Mobile apps (via WebView or API)
- Content creation workflows
- Research and knowledge management systems
- Any platform that needs AI-powered fitness coaching

## Quick Start 🚀

### Prerequisites

Install these first:
1. **Node.js 20+** (LTS recommended)
2. **pnpm** - `npm install -g pnpm`
3. **Docker** (for local PostgreSQL)
4. **Ollama** (for local LLMs) - https://ollama.com

### Installation

```bash
# 1. Clone this repository
git clone https://github.com/SharmaReenaDr/MagicAi.git
cd MagicAi

# 2. Install dependencies
pnpm install

# 3. Start PostgreSQL
pnpm docker:pg

# 4. Configure environment (.env file created automatically)
# Edit .env and set:
# - OLLAMA_BASE_URL=http://localhost:11434/api
# - BETTER_AUTH_SECRET=<random_string>
# - POSTGRES_URL=postgres://neo_user:neo_password@localhost:5432/neocortex_db

# 5. Run database migrations
pnpm db:migrate

# 6. Start the app
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

**For detailed setup instructions**, see the [Fitness Life Mantra Setup Guide](./docs/FLM-SETUP.md).

### Pull Local Models (Optional)

```bash
# Pull recommended models for offline use
ollama pull llama3.1
ollama pull llama3.2:3b
ollama pull qwen2.5-coder:7b
```

## Key Features

### 🧘‍♀️ Fitness Life Mantra Coach

Create custom AI agents with fitness-focused system prompts:
- Daily workout routines and movement ideas
- Breathwork and meditation guidance
- Emotional wellness check-ins
- Content creation for fitness influencers

### 🎙️ Real-time Voice Assistant

Talk naturally to your AI coach with full MCP tool integration for hands-free guidance during workouts.

### 🔧 MCP Tool Integration

Connect external tools and services:
- Web search for fitness research
- Data visualization for progress tracking
- Custom workflows for content creation
- API integrations with fitness apps

### 🤖 Custom Agents

Build specialized assistants:
- **FLM Coach** - General fitness and wellness guidance
- **Content Creator** - Script and caption generation
- **Nutrition Advisor** - Meal planning and dietary advice
- **Movement Specialist** - Dance, yoga, and mobility routines

### 🌐 Embeddable Integration

MagicAi can be integrated into any website or application:

**Option 1: Iframe Embed**
```html
<iframe src="http://your-magic-ai-host:3000" width="100%" height="600px"></iframe>
```

**Option 2: HTTP API** (Coming Soon)
```javascript
const response = await fetch('/api/flm-assistant', {
  method: 'POST',
  body: JSON.stringify({
    messages: [...],
    agent: 'flm_coach'
  })
});
```

## Environment Variables

The `.env` file is automatically created during installation. Key variables:

```env
# === LLM Providers ===
OLLAMA_BASE_URL=http://localhost:11434/api
OPENAI_API_KEY=your_key_here (optional)
ANTHROPIC_API_KEY=your_key_here (optional)
GOOGLE_GENERATIVE_AI_API_KEY=your_key_here (optional)

# === Auth ===
BETTER_AUTH_SECRET=your_random_secret
BETTER_AUTH_URL=http://localhost:3000

# === Database ===
POSTGRES_URL=postgres://neo_user:neo_password@localhost:5432/neocortex_db

# === Tools (Optional) ===
EXA_API_KEY=your_exa_key (for web search)
```

## 📘 Guides

Comprehensive documentation for using and extending MagicAi:

#### [🏋️ Fitness Life Mantra Setup Guide](./docs/FLM-SETUP.md)
- Complete setup instructions for local development
- Creating the FLM coach agent
- Integration strategies

#### [🔌 MCP Server Setup & Tool Testing](./docs/tips-guides/mcp-server-setup-and-tool-testing.md)
- How to add and configure MCP servers
- Testing custom tools

#### [🐳 Docker Hosting Guide](./docs/tips-guides/docker.md)
- Self-host with Docker for production

#### [🎯 System Prompts & Chat Customization](./docs/tips-guides/system-prompts-and-customization.md)
- Personalize your AI coach's personality and behavior

#### [🔐 OAuth Sign-In Setup](./docs/tips-guides/oauth.md)
- Configure Google, GitHub, Microsoft OAuth

#### [🗂️ File Storage Drivers](./docs/tips-guides/file-storage.md)
- Configure cloud storage for uploads

## Built on Neocortex

MagicAi is built on the **Neocortex** open-source framework created by [Ankit Yadav](https://github.com/ankityadavv2014).

Neocortex provides:
- Multi-provider LLM integration (OpenAI, Anthropic, Google, xAI, Ollama)
- Model Context Protocol (MCP) support
- Visual workflow builder
- Authentication with Better Auth
- Database ORM with Drizzle
- Next.js 15 + Vercel AI SDK

**Original Project:** [github.com/ankityadavv2014/neocortex](https://github.com/ankityadavv2014/neocortex)

All core architecture, database schema, and internal code structure remain unchanged. MagicAi adds Fitness Life Mantra branding and use-case-specific customizations on top of this solid foundation.

## Roadmap

Planned enhancements for MagicAi:

- [ ] **FLM Knowledge Base** - RAG integration with fitness research and FLM content
- [ ] **Progress Tracking** - Visual dashboards for fitness metrics
- [ ] **Meal Planning** - AI-generated nutrition plans
- [ ] **Workout Library** - Searchable movement database
- [ ] **Social Features** - Share routines and achievements
- [ ] **Mobile App** - Native iOS/Android experience
- [ ] **Calendar Integration** - Sync with Google Calendar, Apple Health

## 💖 Support

If MagicAi helps your fitness journey, please:
- ⭐ **Star** this repository
- 🐛 **Report** bugs and suggest features
- 🤝 **Contribute** improvements (see [Contributing Guide](./CONTRIBUTING.md))

**Also support the original Neocortex project:**
- ⭐ Star [ankityadavv2014/neocortex](https://github.com/ankityadavv2014/neocortex)
- 💰 [Sponsor Neocortex development](https://github.com/sponsors/cgoinglove)

## 🙌 Contributing

We welcome contributions! Please read our [Contributing Guide](./CONTRIBUTING.md) before submitting Pull Requests.

For language translations, see [messages/language.md](./messages/language.md).

## License

MIT License - see [LICENSE](./LICENSE) for details.

Built with ❤️ for the Fitness Life Mantra community, powered by Neocortex.
