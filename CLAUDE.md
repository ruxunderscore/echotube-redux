# EchoTube

Lightweight, self-hosted bot that monitors YouTube channels/playlists via RSS for new videos matching keywords, then posts notifications to Discord webhooks. Single Docker container, Node.js 20+, zero API keys needed.

## Project Structure

```
src/
  index.js      - Entry point, polling loop, signal handling
  config.js     - Environment variable validation
  logger.js     - Structured JSON logging
  youtube.js    - RSS feed fetching (native fetch + AbortController)
  discord.js    - Rate-limited webhook queue (5 req/2s)
  cache.js      - Video ID tracking with timestamp recovery
  test.js       - Test mode validation
```

## Key Design Decisions

- **RSS only, no YouTube API** - Uses public feeds (`/feeds/videos.xml?channel_id=` / `?playlist_id=`), limited to 25 most recent videos per feed
- **Single dependency** - `@rowanmanning/feed-parser` for XML; everything else is native Node.js 20+ (fetch, ESM, AbortController)
- **Queue-based Discord rate limiting** - Strict 5 requests per 2 seconds with exponential backoff retry
- **Ultra-clean embeds** - Channel name, video title, large thumbnail only. No footer, timestamp, or branding.
- **Initial run behavior** - First startup marks all existing videos as seen (no spam). `ET_DEVELOPMENT_MODE=true` posts one video per source instead.

## Environment Variables

All config is via env vars. See `.env.example` for the full reference. Key ones:
- `ET_DISCORD_WEBHOOK_URLS` - Comma-separated webhook URLs (plural, supports multiple servers)
- `ET_CHANNEL_IDS` / `ET_PLAYLIST_IDS` - Content sources (at least one required)
- `ET_KEYWORDS` / `ET_MATCH_TYPE` - Filtering (any/all)
- `ET_TEST_MODE=true` - Fetches videos but never posts to Discord
- `ET_DEVELOPMENT_MODE=true` - Posts only one video per source

## Development

```bash
npm install
ET_TEST_MODE=true ET_CHANNEL_IDS=UCxxxxxx ET_KEYWORDS=test npm start
```

Docker Compose uses `.env` for secrets (gitignored). Three profiles:
- Default (`echotube`) - production, pulls from Docker Hub
- `test` profile - test mode, no Discord posting
- `dev` profile - builds locally, polls every 60s, development mode

## CI/CD

GitHub Actions in `.github/workflows/`:
- `docker-build.yml` - Builds and pushes to GHCR + Docker Hub on tag push
- `test.yml` - Runs tests

Docker Hub secrets needed: `DOCKERHUB_USERNAME`, `DOCKERHUB_TOKEN`

## Common Pitfalls

- Monitoring both a channel AND its playlists causes duplicate notifications
- RSS feeds only have 25 videos - high-volume channels need shorter poll intervals
- Private playlists are not accessible via RSS
