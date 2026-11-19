# Contributing to EchoTube

Thanks for your interest in contributing!

## Reporting Bugs

Open a [bug report](https://github.com/ruxunderscore/echotube-redux/issues/new?template=bug_report.yml) with:
- What happened vs what you expected
- Steps to reproduce (with sanitized config - remove webhook URLs)
- Logs from running with `ET_LOG_LEVEL=DEBUG`

## Suggesting Features

Open a [feature request](https://github.com/ruxunderscore/echotube-redux/issues/new?template=feature_request.yml) describing what you'd like and why.

## Code Contributions

1. Fork the repo and create a branch
2. Make your changes
3. Test with `ET_TEST_MODE=true` and/or build the Docker image
4. Open a pull request

### Local Development

```bash
npm install
cp .env.example .env
# Edit .env with your test config

ET_TEST_MODE=true npm start          # Safe testing
ET_LOG_LEVEL=DEBUG npm start         # Verbose output
docker build -t echotube:test .      # Build container
```
