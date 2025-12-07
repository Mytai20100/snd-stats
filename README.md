# snd-stats

Fast, lightweight API for generating GitHub statistics as SVG badges.

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Node](https://img.shields.io/badge/node-22-brightgreen)](package.json)
[![Version](https://img.shields.io/badge/version-0.2beta-orange)](package.json)

## Features

- SVG badges with real-time GitHub data
- User and repository statistics
- Multi-platform support (GitHub, Twitter, YouTube, Discord)
- In-memory caching with configurable TTL
- Rate limiting (60 requests/minute)
- Zero external dependencies for core functionality

## Quick Start
### Installation

```bash
# Clone repository
git clone https://github.com/mytai20100/snd-stats.git
cd snd-stats

# Install dependencies
npm install

# Start server
npm start
```


### Environment Variables
```env
PORT=3444
GITHUB_TOKEN=your_github_token
API_KEY=your_api_key
CACHE_TTL_SECONDS=300
```

## API Documentation

Base URL: `https://api.snd.qzz.io`(my host) or `https://snd-stats.vercel.app/`

### User Statistics
```
GET /stats/user/:username
```

Parameters:
- `format` - svg or json (default: svg)
- `width` - Card width (default: 495)
- `height` - Card height (default: 250)
- `theme` - Color theme (default, dark, blue, green, etc.)
- `chart_type` - line, bar, area, smooth (default: line)
- `show_frameworks` - true or false (default: true)
- `top_langs` - Number of languages to show (default: 5)

Example:
```html
![Stats](https://api.snd.qzz.io/stats/user/torvalds)
```
![Stats](https://api.snd.qzz.io/stats/user/torvalds)
### Repository Statistics
```
GET /stats/repo/:owner/:repo
```

Parameters:
- `format` - svg or json (default: svg)
- `width` - Card width (default: 495)
- `height` - Card height (default: 280)
- `show_chart` - true or false (default: true)
- `chart_type` - line, bar, area, smooth

Example:
```html
![Repo](https://api.snd.qzz.io/stats/repo/facebook/react)
```
![Repo](https://api.snd.qzz.io/stats/repo/facebook/react)
### Profile Card
```
GET /profile?username=:username
```

Parameters:
- `data` - Comma-separated fields: followers,repositories,stars,commits
- `theme` - Color theme
- `url` - Custom avatar URL
- `background` - Background image URL

Example:
```html
![Profile](https://api.snd.qzz.io/profile?username=mytai20100)
```
![Profile](https://api.snd.qzz.io/profile?username=mytai20100)
### Custom Badges
```
GET /badge/custom
```

Parameters:
- `text` - Badge text
- `width` - Badge width (default: 250)
- `height` - Badge height (default: 80)
- `bg_color` - Background color (hex)
- `text_color` - Text color (hex)
- `theme` - blue, green, red, purple, gray, black, white

Example:
```html
![Badge](https://api.snd.qzz.io/badge/custom?text=Hello%20World&theme=blue)
```
![Badge](https://api.snd.qzz.io/custom?text=Hello%20World&theme=blue)
### Static Badge
```
GET /badge/static?owner=:owner&repo=:repo
```

Compact badge showing language and stars.

### Rank Badge
```
GET /badge/rank?owner=:owner&repo=:repo&list=:repos
```

Compare repository ranking by stars.

Example:
```html
![Rank](https://api.snd.qzz.io/badge/rank?owner=Mytai20100&repo=freeroot-jar)
```
![Rank](https://api.snd.qzz.io/badge/rank?owner=Mytai20100&repo=freeroot-jar)
### Social Media Stats
```
GET /social/:platform/:identifier
```

Platforms: twitter, youtube

Example:
```html
![Twitter](https://api.snd.qzz.io/social/twitter/github)
```
![Twitter](https://api.snd.qzz.io/social/twitter/github)
### OS Badges
```
GET /os/:osname
```

Supported: Linux, Ubuntu, Debian, Windows, macOS, Android, iOS

### Framework Badges
```
GET /framework/:name
```

Supported: React, Vue, Angular, Next.js, Django, Flask, Spring, Laravel, and more

### Social Icons
```
GET /icon/social/:name
```

Platforms: github, twitter, youtube, discord, linkedin, instagram, facebook

Parameters:
- `size` - Icon size (default: 40)
- `color` - Icon color (hex)
- Example:
- ![GitHub](https://api.snd.qzz.io/icon/social/github?size=32&color=333)
- ![Twitter](https://api.snd.qzz.io/icon/social/twitter?size=32&color=1DA1F2)
- ![YouTube](https://api.snd.qzz.io/icon/social/youtube?size=32&color=FF0000)
### Available Icons
```
GET /icons/list
```

Returns JSON with all available icons by category.

## Color Themes

Available themes:
- default - White background, blue accent
- dark - Dark background, light text
- blue - Blue theme
- green - Green theme
- purple - Purple theme
- red - Red theme
- orange - Orange theme
- pink - Pink theme
- gray - Gray theme
- black - Black theme
- minimal - Minimal white
- minimal-dark - Minimal dark

## Chart Types

- `line` - Default line chart
- `bar` - Bar chart
- `area` - Area chart with gradient
- `smooth` - Smooth curved line

## Color Formats

Supported formats:
- Hex: `ffffff` or `#ffffff`
- RGB: `255,255,255`

## Discord Integration

Update server stats:
```bash
POST /discord/update
Headers: X-API-Key: your_api_key
Body: {
  "serverId": "123456789",
  "serverName": "Server Name",
  "members": 1234
}
```

Get badge:
```
GET /discord/badge/:serverId
```

## Rate Limiting

- 60 requests per minute per IP
- Cached responses for 5 minutes
- GitHub API: 5000 requests/hour with token

## Deployment

### Docker
```bash
docker build -t snd-stats .
docker run -d -p 3444:3444 -e GITHUB_TOKEN=token snd-stats
```

### PM2
```bash
npm run build
pm2 start dist/app.js --name snd-stats
```

## Health Check
```
GET /health
```

Returns server status and cache information.

## Error Handling

Standard HTTP status codes:
- 200 - Success
- 400 - Bad request
- 404 - Not found
- 429 - Rate limit exceeded
- 500 - Server error

## Security

- API key required for sensitive endpoints
- Rate limiting to prevent abuse
- Input validation on all parameters
- CORS enabled for cross-origin requests

## License

MIT License - see LICENSE file

## Links

- Documentation: https://github.com/mytai20100/snd-stats
- Issues: https://github.com/mytai20100/snd-stats/issues
- API Status: https://api.snd.qzz.io/health

<div align="center">

**[⬆ Back to Top](#-snd-stats-api-v21)**

Made with ❤️ by [mytai20100](https://github.com/mytai20100)

</div>
