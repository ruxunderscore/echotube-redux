# Changelog

All notable changes to EchoTube will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- GitHub Actions workflows for Docker builds and testing
- Issue templates, PR template, `CONTRIBUTING.md`, `LICENSE`
- `.env.example` reference file and `.env` support in docker-compose

### Changed
- Merged development (`echotube-dev`) and public (`echotube`) repositories into a single repo
- Docker Compose now uses `.env` file for secrets instead of hardcoded values
- Trimmed `CLAUDE.md` from ~13KB to ~2.5KB, removing duplication with README

### Removed
- `docs/EchoTube_SRS.md` and `IMPLEMENTATION_PLAN.md` (completed, no longer needed)
- `public-release/` staging directory (no longer needed with single repo)
- Obsolete `version` field from docker-compose

## [1.0.1] - 2025-01-23

### Added
- Multi-server support: post notifications to multiple Discord servers simultaneously
- Webhook-specific success/failure tracking in logs
- `MultiDiscordWebhook` class with independent rate limiting queues per webhook

### Changed
- `ET_DISCORD_WEBHOOK_URL` renamed to `ET_DISCORD_WEBHOOK_URLS` (comma-separated, backward compatible)
- Individual webhook failures no longer affect other webhooks

## [1.0.0] - 2025-01-22

### Added
- YouTube RSS monitoring for channels and playlists (no API keys needed)
- Case-insensitive keyword matching with "any" or "all" modes
- Ultra-clean Discord embeds with large thumbnails (1280x720)
- Smart initial run behaviour: marks existing videos as seen without posting
- Development mode (`ET_DEVELOPMENT_MODE=true`): posts one video per source
- Test mode (`ET_TEST_MODE=true`): validates configuration without posting
- Structured JSON logging with configurable levels
- Queue-based Discord rate limiting (5 requests/2 seconds) with exponential backoff
- Timestamp-based cache recovery for graceful failure handling
- Multi-stage Alpine Docker build with non-root user (echotube:1001)

### Security
- Runs as non-root dedicated user in container
- Single external dependency (@rowanmanning/feed-parser)
- Comprehensive environment variable validation
- No API keys or authentication required

[Unreleased]: https://github.com/richardthornton/echotube/compare/v1.0.1...HEAD
[1.0.1]: https://github.com/richardthornton/echotube/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/richardthornton/echotube/releases/tag/v1.0.0
