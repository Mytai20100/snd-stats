# Snd-stats 

Fast TypeScript API for generating GitHub repo/user stats as SVG badges and cards. Includes Discord bot integration.

## Features

- SVG badges with hand-drawn style
- User and repository statistics
- Ranking system
- Discord server member count tracking
- In-memory caching to avoid rate limits
- Single-file architecture for easy deployment

## Live API

Base URL: `https://api.snd.qzz.io`

## Quick Start

### Install dependencies
```bash
npm install
```

### Configure environment

Copy `.env.example` to `.env` and fill in your values:
```bash
cp .env.example .env
```

### Run locally
```bash
npm run dev
```

### Build for production
```bash
npm run build
npm start
```

### Run Discord bot
```bash
npm run bot
```

## API Endpoints

### Health Check
```
GET /health
```

**Example:**
```bash
curl https://api.snd.qzz.io/health
```

### User Stats
```
GET /stats/user/:username?format=svg|json
```

**Examples:**
```bash
# Get SVG card
curl https://api.snd.qzz.io/stats/user/torvalds

# Get JSON data
curl https://api.snd.qzz.io/stats/user/torvalds?format=json
```

### Repository Stats
```
GET /stats/repo/:owner/:repo?format=svg|json
```

**Examples:**
```bash
# Get SVG card
curl https://api.snd.qzz.io/stats/repo/facebook/react

# Get JSON data
curl https://api.snd.qzz.io/stats/repo/facebook/react?format=json
```

### Badges

**Flex badge (full card):**
```
GET /badge/flex?owner=:owner&repo=:repo
GET /badge/flex?user=:username
```

**Static badge (compact):**
```
GET /badge/static?owner=:owner&repo=:repo
```

**Rank badge:**
```
GET /badge/rank?owner=:owner&repo=:repo&list=:repo1,:repo2,:repo3
```

**Examples:**
```bash
# Flex card for repository
curl https://api.snd.qzz.io/badge/flex?owner=facebook&repo=react

# Flex card for user
curl https://api.snd.qzz.io/badge/flex?user=torvalds

# Static badge
curl https://api.snd.qzz.io/badge/static?owner=facebook&repo=react

# Rank badge
curl https://api.snd.qzz.io/badge/rank?owner=facebook&repo=react&list=vuejs/vue,angular/angular,facebook/react
```

### Ranking
```
GET /rank/repo?owner=:owner&repo=:repo&list=:repo1,:repo2,:repo3
```

**Example:**
```bash
curl "https://api.snd.qzz.io/rank/repo?owner=facebook&repo=react&list=vuejs/vue,angular/angular,facebook/react"
```

### Discord Integration

**Update server stats:**
```
POST /discord/update
Headers: x-api-key: your_api_key
Body: { "serverId": "123", "serverName": "My Server", "members": 1500 }
```

**Get server badge:**
```
GET /discord/badge/:serverId
```

**Examples:**
```bash
# Update Discord server stats
curl -X POST https://api.snd.qzz.io/discord/update \
  -H "Content-Type: application/json" \
  -H "x-api-key: your_api_key" \
  -d '{"serverId":"123456789","serverName":"My Discord","members":1500}'

# Get Discord server badge
curl https://api.snd.qzz.io/discord/badge/123456789
```

## README Integration Examples

### Static Badge

Show repository stats in a compact badge:
```markdown
![Repo Stats](https://api.snd.qzz.io/badge/static?owner=facebook&repo=react)
```

![Repo Stats](https://api.snd.qzz.io/badge/static?owner=facebook&repo=react)

---

### Flex Card - Repository

Display full repository statistics card:
```markdown
![React Stats](https://api.snd.qzz.io/stats/repo/facebook/react)
```

![React Stats](https://api.snd.qzz.io/stats/repo/facebook/react)

---

### Flex Card - User

Display user GitHub statistics:
```markdown
![User Stats](https://api.snd.qzz.io/stats/user/torvalds)
```

![User Stats](https://api.snd.qzz.io/stats/user/torvalds)

---

### Rank Badge

Show repository rank among competitors:
```markdown
![Rank](https://api.snd.qzz.io/badge/rank?owner=facebook&repo=react&list=vuejs/vue,angular/angular,facebook/react)
```

![Rank](https://api.snd.qzz.io/badge/rank?owner=facebook&repo=react&list=vuejs/vue,angular/angular,facebook/react)

---

### Discord Server Badge

Display Discord server member count:
```markdown
![Discord Members](https://api.snd.qzz.io/discord/badge/YOUR_SERVER_ID)
```

---

## Advanced Examples

### Custom Repository Comparison

Compare your repo against popular alternatives:
```markdown

![My Project](https://api.snd.qzz.io/badge/flex?owner=myusername&repo=myproject)


![Rank](https://api.snd.qzz.io/badge/rank?owner=myusername&repo=myproject&list=competitor1/repo1,competitor2/repo2,myusername/myproject)
```

### Multiple User Cards

Show stats for your team members:
```markdown
### Core Team

![Alice](https://api.snd.qzz.io/badge/flex?user=alice)
![Bob](https://api.snd.qzz.io/badge/flex?user=bob)
![Charlie](https://api.snd.qzz.io/badge/flex?user=charlie)
```

### Combined Stats Dashboard

Create a complete stats dashboard in your README:
```markdown
## 📊 Project Statistics

### Repository Stats
![Repo Card](https://api.snd.qzz.io/stats/repo/myorg/myrepo)

### Quick Stats
![Static Badge](https://api.snd.qzz.io/badge/static?owner=myorg&repo=myrepo)

### Community
![Discord](https://api.snd.qzz.io/discord/badge/YOUR_SERVER_ID)

### Ranking
![Rank](https://api.snd.qzz.io/badge/rank?owner=myorg&repo=myrepo&list=competitor1/repo,competitor2/repo,myorg/myrepo)
```

## Docker Deployment
```bash
# Build image
docker build -t repo-stats-api .

# Run container
docker run -d \
  -p 3000:3000 \
  -e GITHUB_TOKEN=your_github_token \
  -e API_KEY=your_secret_key \
  --name repo-stats-api \
  repo-stats-api
```

## Environment Variables

| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `PORT` | Server port | `3000` | No |
| `GITHUB_TOKEN` | GitHub personal access token | - | Recommended |
| `API_KEY` | Secret key for Discord endpoint | `default-secret-key` | Yes (production) |
| `CACHE_TTL_SECONDS` | Cache duration in seconds | `300` | No |
| `DISCORD_TOKEN` | Discord bot token (for bot) | - | Yes (for bot) |
| `API_URL` | API base URL (for bot) | `http://localhost:3000` | Yes (for bot) |

## Rate Limits

- **Without `GITHUB_TOKEN`**: 60 requests/hour per IP
- **With `GITHUB_TOKEN`**: 5,000 requests/hour
- **Caching**: Default 5-minute TTL reduces GitHub API calls

## Discord Bot Setup

1. Create a Discord bot at [Discord Developer Portal](https://discord.com/developers/applications)
2. Enable `Server Members Intent` in Bot settings
3. Copy bot token and add to `.env`
4. Run the bot:
```bash
npm run bot
```

The bot will automatically send member count updates to the API when:
- Bot joins a new server
- A member joins the server
- A member leaves the server

## Supported Languages

The API includes built-in icons for these languages:

- JavaScript, TypeScript
- Python, Java, Go, Rust
- C, C++, C#
- PHP, Ruby, Swift, Kotlin
- Lua, Shell
- HTML, CSS

Unknown languages display a generic code icon.

## Features

### Hand-Drawn Style

All SVG badges use a sketchy, hand-drawn aesthetic:
- Irregular stroke paths
- Dashed strokes
- Imperfect circles
- Stylized sparklines

### Smart Caching

Intelligent caching system to minimize GitHub API calls:
- User stats cached for 5 minutes
- Repository stats cached for 5 minutes
- Automatic cache invalidation

### Error Handling

Graceful error responses:
- GitHub rate limit errors
- Invalid repository/user names
- Missing parameters
- Authentication failures

## Testing Locally
```bash
# Health check
curl http://localhost:3000/health

# User stats (JSON)
curl http://localhost:3000/stats/user/torvalds?format=json

# Repo stats (SVG)
curl http://localhost:3000/stats/repo/facebook/react > badge.svg

# Static badge
curl http://localhost:3000/badge/static?owner=facebook&repo=react > static.svg

# Discord update
curl -X POST http://localhost:3000/discord/update \
  -H "Content-Type: application/json" \
  -H "x-api-key: your_api_key" \
  -d '{"serverId":"123","serverName":"Test Server","members":100}'
```

## Production Deployment

### Using PM2
```bash
npm run build
pm2 start dist/app.js --name repo-stats-api
```

### Using systemd

Create `/etc/systemd/system/repo-stats-api.service`:
```ini
[Unit]
Description=Repo Stats API
After=network.target

[Service]
Type=simple
User=www-data
WorkingDirectory=/path/to/repo-stats-api
ExecStart=/usr/bin/node dist/app.js
Restart=on-failure
Environment=NODE_ENV=production
Environment=PORT=3000
Environment=GITHUB_TOKEN=your_token
Environment=API_KEY=your_key

[Install]
WantedBy=multi-user.target
```

Then:
```bash
sudo systemctl enable repo-stats-api
sudo systemctl start repo-stats-api
```

## Security Notes

- Always use a strong `API_KEY` in production
- Keep `GITHUB_TOKEN` secret
- Use HTTPS in production (recommended: nginx reverse proxy)
- Validate all user inputs
- Rate limit requests at reverse proxy level

## Contributing

Pull requests are welcome! Please ensure:
- Code follows existing style
- Comments are in English
- All endpoints are tested
- Documentation is updated

## License

MIT

## Support

For issues and questions:
- GitHub Issues: [https://github.com/Mytai20100/snd-stats/issues]
- API Status: https://api.snd.qzz.io/health

---

❤️ Made by mytai20100 
