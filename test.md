# 🎨 snd-stats - GitHub Stats API v2.0

> Clean, fast, production-ready API for generating beautiful GitHub stats badges with hand-drawn styling, framework detection, and social media integration.

[![API Status](https://api.snd.qzz.io/health)](https://api.snd.qzz.io)
[![GitHub](https://img.shields.io/badge/GitHub-mytai20100%2Fsnd--stats-blue)](https://github.com/mytai20100/snd-stats)

**Live API:** `https://api.snd.qzz.io`

---

## ✨ Features

- 🎯 **GitHub Stats** - User and repository statistics with beautiful SVG cards
- 🎨 **Hand-Drawn Style** - Sketchy, artistic badges with animated sparklines
- 🧩 **Framework Detection** - Auto-detect React, Vue, Next.js, Django, and 25+ frameworks
- 💻 **OS Icons** - Support for Linux, Windows, macOS, and 15+ distributions
- 🌐 **Social Media** - Twitter/X and YouTube integration
- 🎨 **Fully Customizable** - Colors (RGB/HEX), sizes, and styling options
- ⚡ **Fast & Cached** - Smart caching system with 5-minute TTL
- 🔒 **Rate Limited** - 60 requests/minute per IP
- 🐳 **Docker Ready** - Easy deployment with Docker

---

## 📚 Table of Contents

- [Quick Start](#-quick-start)
- [API Endpoints](#-api-endpoints)
- [Badge Types](#-badge-types)
- [Customization Options](#-customization-options)
- [README Examples](#-readme-examples)
- [Social Media Integration](#-social-media-integration)
- [Framework & OS Badges](#-framework--os-badges)
- [Advanced Usage](#-advanced-usage)
- [Deployment](#-deployment)
- [Environment Variables](#-environment-variables)

---

## 🚀 Quick Start

### Installation
```bash
# Clone repository
git clone https://github.com/mytai20100/snd-stats.git
cd snd-stats

# Install dependencies
npm install

# Create .env file
cp .env.example .env

# Edit .env with your tokens
nano .env

# Run development server
npm run dev

# Or build for production
npm run build
npm start
```

### Docker Deployment
```bash
docker build -t snd-stats .
docker run -d -p 3444:3444 \
  -e GITHUB_TOKEN=your_token \
  -e API_KEY=your_secret \
  --name snd-stats \
  snd-stats
```

---

## 🎯 API Endpoints

### Health Check
```http
GET /health
```

**Response:**
```json
{
  "status": "ok",
  "timestamp": 1701234567890,
  "version": "2.0.0",
  "cache_size": 42,
  "uptime": 3600.5
}
```

---

### User Statistics
```http
GET /stats/user/:username?format=svg|json
```

**Parameters:**
- `username` - GitHub username (required)
- `format` - Response format: `svg` (default) or `json`
- `width` - Card width (default: 495)
- `height` - Card height (default: 250)
- `bg_color` - Background color (hex/rgb)
- `text_color` - Text color (hex/rgb)
- `accent_color` - Accent color (hex/rgb)
- `show_frameworks` - Show frameworks: `true` (default) or `false`

**Examples:**
```bash
# Default SVG card
curl https://api.snd.qzz.io/stats/user/torvalds

# JSON format
curl https://api.snd.qzz.io/stats/user/torvalds?format=json

# Custom colors
curl "https://api.snd.qzz.io/stats/user/torvalds?bg_color=1a1b27&text_color=ffffff&accent_color=7aa2f7"

# Custom size
curl "https://api.snd.qzz.io/stats/user/torvalds?width=600&height=300"
```

---

### Repository Statistics
```http
GET /stats/repo/:owner/:repo?format=svg|json
```

**Parameters:**
- `owner` - Repository owner (required)
- `repo` - Repository name (required)
- `format` - Response format: `svg` (default) or `json`
- `width` - Card width (default: 495)
- `height` - Card height (default: 280)
- `bg_color` - Background color
- `text_color` - Text color
- `accent_color` - Accent color
- `show_frameworks` - Show frameworks: `true` (default) or `false`
- `show_chart` - Show activity charts: `true` (default) or `false`

**Examples:**
```bash
# Default card
curl https://api.snd.qzz.io/stats/repo/facebook/react

# Dark theme
curl "https://api.snd.qzz.io/stats/repo/facebook/react?bg_color=0d1117&text_color=c9d1d9&accent_color=58a6ff"

# Without charts
curl "https://api.snd.qzz.io/stats/repo/facebook/react?show_chart=false"

# JSON format
curl https://api.snd.qzz.io/stats/repo/facebook/react?format=json
```

---

## 🏷️ Badge Types

### 1. Flex Badge (Full Card)
```http
GET /badge/flex?owner=:owner&repo=:repo
GET /badge/flex?user=:username
```

**For Repository:**
```markdown
![React Stats](https://api.snd.qzz.io/badge/flex?owner=facebook&repo=react)
```

![React Stats](https://api.snd.qzz.io/badge/flex?owner=facebook&repo=react)

**For User:**
```markdown
![User Stats](https://api.snd.qzz.io/badge/flex?user=torvalds)
```

![User Stats](https://api.snd.qzz.io/badge/flex?user=torvalds)

---

### 2. Static Badge (Compact)
```http
GET /badge/static?owner=:owner&repo=:repo
```

**Example:**
```markdown
![Repo Badge](https://api.snd.qzz.io/badge/static?owner=facebook&repo=react)
```

![Repo Badge](https://api.snd.qzz.io/badge/static?owner=facebook&repo=react)

**Custom Colors:**
```markdown
![Custom Badge](https://api.snd.qzz.io/badge/static?owner=facebook&repo=react&label_color=24292e&value_color=0366d6)
```

---

### 3. Rank Badge
```http
GET /badge/rank?owner=:owner&repo=:repo&list=:repo1,:repo2,:repo3
```

**Example:**
```markdown
![Rank](https://api.snd.qzz.io/badge/rank?owner=facebook&repo=react&list=vuejs/vue,angular/angular,facebook/react,sveltejs/svelte)
```

![Rank](https://api.snd.qzz.io/badge/rank?owner=facebook&repo=react&list=vuejs/vue,angular/angular,facebook/react,sveltejs/svelte)

**Custom Colors:**
```markdown
![Custom Rank](https://api.snd.qzz.io/badge/rank?owner=facebook&repo=react&list=vuejs/vue,angular/angular,facebook/react&bg_color=28a745&text_color=ffffff)
```

---

## 🎨 Customization Options

### Color Formats

The API supports multiple color formats:

**Hex Format:**
```
?bg_color=2f80ed
?bg_color=#2f80ed
```

**RGB Format:**
```
?bg_color=47,128,237
?bg_color=rgb(47,128,237)
```

### Size Options
```
?width=600&height=400
```

### Complete Example
```markdown
![Custom Stats](https://api.snd.qzz.io/stats/user/mytai20100?width=600&height=300&bg_color=0d1117&text_color=c9d1d9&accent_color=58a6ff&show_frameworks=true)
```

---

## 📖 README Examples

### Basic Setup
```markdown
# My Project

![GitHub Stats](https://api.snd.qzz.io/stats/repo/myusername/myproject)

## Stats
![User Stats](https://api.snd.qzz.io/stats/user/myusername)
```

---

### Dark Theme
```markdown
## 📊 GitHub Statistics



![Stats](https://api.snd.qzz.io/stats/user/mytai20100?bg_color=0d1117&text_color=c9d1d9&accent_color=58a6ff)

![Repo](https://api.snd.qzz.io/stats/repo/mytai20100/snd-stats?bg_color=0d1117&text_color=c9d1d9&accent_color=58a6ff)


```

---

### Side by Side
```markdown

  
  

```

---

### Full Dashboard
```markdown
## 📊 Project Dashboard

### Repository Stats
![Repo Card](https://api.snd.qzz.io/stats/repo/myorg/myrepo?width=495&height=280)

### Quick Stats

  
  


### Team Members

  
  

```

---

## 🌐 Social Media Integration

### Twitter/X Stats
```http
GET /social/twitter/:username?format=svg|json
GET /social/x/:username?format=svg|json
```

**Example:**
```markdown
![Twitter Stats](https://api.snd.qzz.io/social/twitter/elonmusk)
```

**Custom Colors:**
```markdown
![Twitter](https://api.snd.qzz.io/social/twitter/elonmusk?bg_color=000000&accent_color=1DA1F2&width=320&height=130)
```

---

### YouTube Stats
```http
GET /social/youtube/:channelId?format=svg|json
```

**Example:**
```markdown
![YouTube Stats](https://api.snd.qzz.io/social/youtube/UCX6OQ3DkcsbYNE6H8uQQuVA)
```

**Note:** Requires `YOUTUBE_API_KEY` environment variable.

---

## 🧩 Framework & OS Badges

### Framework Badge
```http
GET /framework/:name?width=150&height=50&bg_color=ffffff&text_color=333333
```

**Supported Frameworks:**
- React, Vue, Angular, Next.js, Nuxt.js
- Svelte, Django, Flask, Express.js
- Laravel, Spring, ASP.NET, Rails
- Nest.js, Gatsby, Astro, Remix
- SolidJS, Qwik, Tauri, Electron

**Examples:**
```markdown
![React](https://api.snd.qzz.io/framework/React)
![Vue](https://api.snd.qzz.io/framework/Vue)
![Django](https://api.snd.qzz.io/framework/Django)
![Next.js](https://api.snd.qzz.io/framework/Next.js)
```

<p align="center">
  <img src="https://api.snd.qzz.io/framework/React" />
  <img src="https://api.snd.qzz.io/framework/Vue" />
  <img src="https://api.snd.qzz.io/framework/Django" />
</p>

---

### OS/Platform Badge
```http
GET /os/:osname?width=150&height=50&bg_color=ffffff&text_color=333333
```

**Supported Operating Systems:**
- **Linux:** Ubuntu, Debian, Fedora, CentOS, Arch Linux, Manjaro, openSUSE, Red Hat, Kali Linux, Alpine
- **Windows:** Windows, Windows Server
- **Apple:** macOS, iOS
- **Mobile:** Android

**Examples:**
```markdown
![Ubuntu](https://api.snd.qzz.io/os/Ubuntu)
![Windows](https://api.snd.qzz.io/os/Windows)
![macOS](https://api.snd.qzz.io/os/macOS)
![Arch Linux](https://api.snd.qzz.io/os/Arch%20Linux)
```

<p align="center">
  <img src="https://api.snd.qzz.io/os/Ubuntu" />
  <img src="https://api.snd.qzz.io/os/Windows" />
  <img src="https://api.snd.qzz.io/os/macOS" />
</p>

---

### List All Available Icons
```http
GET /icons/list
```

**Response:**
```json
{
  "languages": ["JavaScript", "TypeScript", "Python", "Java", "Go", ...],
  "frameworks": ["React", "Vue", "Angular", "Django", "Flask", ...],
  "os": ["Linux", "Ubuntu", "Windows", "macOS", "Android", ...]
}
```

---

## 🔥 Advanced Usage

### Repository Ranking
```http
GET /rank/repo?owner=:owner&repo=:repo&list=:repo1,:repo2,:repo3
```

**Example:**
```bash
curl "https://api.snd.qzz.io/rank/repo?owner=facebook&repo=react&list=vuejs/vue,angular/angular,facebook/react,sveltejs/svelte"
```

**Response:**
```json
{
  "repo": "facebook/react",
  "rank": 1,
  "total": 4,
  "leaderboard": [
    { "repo": "facebook/react", "stars": 220000 },
    { "repo": "vuejs/vue", "stars": 207000 },
    { "repo": "angular/angular", "stars": 95000 },
    { "repo": "sveltejs/svelte", "stars": 75000 }
  ]
}
```

---

### Discord Integration

**Update Server Stats:**
```http
POST /discord/update
Headers: x-api-key: your_api_key
Content-Type: application/json

{
  "serverId": "123456789",
  "serverName": "My Discord Server",
  "members": 1500
}
```

**Get Discord Badge:**
```http
GET /discord/badge/:serverId
```

**Example:**
```markdown
![Discord Members](https://api.snd.qzz.io/discord/badge/123456789)
```

**Discord Bot Setup:**
```bash
npm run bot
```

See `src/discord-bot.ts` for bot implementation.

---

## 🎨 Color Themes

### GitHub Dark
```
bg_color=0d1117
text_color=c9d1d9
accent_color=58a6ff
```

### Dracula
```
bg_color=282a36
text_color=f8f8f2
accent_color=bd93f9
```

### Nord
```
bg_color=2e3440
text_color=d8dee9
accent_color=88c0d0
```

### Monokai
```
bg_color=272822
text_color=f8f8f2
accent_color=a6e22e
```

### Solarized Dark
```
bg_color=002b36
text_color=839496
accent_color=268bd2
```

### Tokyo Night
```
bg_color=1a1b26
text_color=a9b1d6
accent_color=7aa2f7
```

---

## 🐳 Deployment

### Docker Compose
```yaml
version: '3.8'

services:
  snd-stats:
    build: .
    ports:
      - "3444:3444"
    environment:
      - PORT=3444
      - GITHUB_TOKEN=${GITHUB_TOKEN}
      - API_KEY=${API_KEY}
      - CACHE_TTL_SECONDS=300
      - TWITTER_BEARER_TOKEN=${TWITTER_BEARER_TOKEN}
      - YOUTUBE_API_KEY=${YOUTUBE_API_KEY}
    restart: unless-stopped
```

### Nginx Reverse Proxy
```nginx
server {
    listen 80;
    server_name api.snd.qzz.io;

    location / {
        proxy_pass http://localhost:3444;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

### PM2 Process Manager
```bash
npm run build
pm2 start dist/app.js --name snd-stats
pm2 save
pm2 startup
```

---

## ⚙️ Environment Variables

Create `.env` file:
```env
# Server Configuration
PORT=3444
NODE_ENV=production

# GitHub API (Required for stats)
GITHUB_TOKEN=ghp_your_github_personal_access_token

# API Security
API_KEY=your_secret_api_key_for_discord_endpoint

# Cache Configuration
CACHE_TTL_SECONDS=300

# Social Media APIs (Optional)
TWITTER_BEARER_TOKEN=your_twitter_bearer_token
YOUTUBE_API_KEY=your_youtube_api_key

# Discord Bot (Optional)
DISCORD_TOKEN=your_discord_bot_token
API_URL=https://api.snd.qzz.io
```

### Getting API Tokens

**GitHub Token:**
1. Go to https://github.com/settings/tokens
2. Generate new token (classic)
3. Select scopes: `public_repo`, `read:user`
4. Copy token to `.env`

**Twitter Bearer Token:**
1. Go to https://developer.twitter.com/
2. Create app and get Bearer Token
3. Add to `.env`

**YouTube API Key:**
1. Go to https://console.cloud.google.com/
2. Enable YouTube Data API v3
3. Create credentials (API Key)
4. Add to `.env`

---

## 📊 Rate Limits

- **Default:** 60 requests per minute per IP
- **With GitHub Token:** 5,000 requests/hour to GitHub API
- **Cache Duration:** 5 minutes (300 seconds)

---

## 🎯 Use Cases

### Personal Portfolio
```markdown
## About Me
![My GitHub Stats](https://api.snd.qzz.io/stats/user/mytai20100?width=600&bg_color=ffffff&accent_color=2f80ed)
```

### Project README
```markdown
## Project Stats
![Repo Stats](https://api.snd.qzz.io/stats/repo/mytai20100/snd-stats?show_chart=true)
```

### Organization Dashboard
```markdown
## Team Performance
![Member 1](https://api.snd.qzz.io/badge/flex?user=alice&width=300)
![Member 2](https://api.snd.qzz.io/badge/flex?user=bob&width=300)
```

### Technology Stack
```markdown
## Built With
![React](https://api.snd.qzz.io/framework/React)
![TypeScript](https://api.snd.qzz.io/framework/TypeScript)
![Node.js](https://api.snd.qzz.io/framework/Express.js)
```

---

## 🛠️ Development
```bash
# Install dependencies
npm install

# Run in development mode
npm run dev

# Build for production
npm run build

# Start production server
npm start

# Run Discord bot
npm run bot

# Run tests (if available)
npm test
```

---

## 📝 API Response Examples

### User Stats (JSON)
```json
{
  "username": "torvalds",
  "name": "Linus Torvalds",
  "avatar": "https://avatars.githubusercontent.com/u/1024025",
  "repos": 25,
  "followers": 180000,
  "following": 0,
  "totalStars": 45000,
  "topLanguage": "C",
  "languages": ["C", "Shell", "Makefile"],
  "frameworks": [],
  "createdAt": "2011-09-03T15:26:22Z"
}
```

### Repository Stats (JSON)
```json
{
  "owner": "facebook",
  "name": "react",
  "fullName": "facebook/react",
  "description": "The library for web and native user interfaces",
  "stars": 220000,
  "forks": 45000,
  "watchers": 6800,
  "language": "JavaScript",
  "openIssues": 1200,
  "commits": 30,
  "updatedAt": "2024-01-15T10:30:00Z",
  "frameworks": ["React"],
  "topics": ["react", "javascript", "ui", "frontend"]
}
```

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details

---

## 🙏 Acknowledgments

- Inspired by [github-readme-stats](https://github.com/anuraghazra/github-readme-stats)
- Icons and styling inspired by various badge services
- Built with TypeScript, Express, and lots of ☕

---

## 📞 Support

- **GitHub Issues:** [github.com/mytai20100/snd-stats/issues](https://github.com/mytai20100/snd-stats/issues)
- **API Status:** [api.snd.qzz.io/health](https://api.snd.qzz.io/health)
- **Author:** [@mytai20100](https://github.com/mytai20100)

---

## 🌟 Star History

[![Star History Chart](https://api.snd.qzz.io/stats/repo/mytai20100/snd-stats)](https://github.com/mytai20100/snd-stats/stargazers)

---

<div align="center">

**Made with ❤️ by [mytai20100](https://github.com/mytai20100)**

![Footer](https://api.snd.qzz.io/badge/static?owner=mytai20100&repo=snd-stats&label_color=24292e&value_color=0366d6)

*Powered by TypeScript, Express & GitHub API*

</div>
