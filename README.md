# Heroku Buildpack: Claude Code

This buildpack installs [Claude Code](https://claude.ai/product/claude-code), Anthropic's agentic coding tool, on Heroku dynos.

## Usage

### 1. Add the buildpack to your app

```bash
heroku buildpacks:add https://github.com/newcompute-ai/heroku-buildpack-claude-code
```

Or set it in your `app.json`:

```json
{
  "buildpacks": [
    { "url": "heroku/nodejs" },
    { "url": "https://github.com/newcompute-ai/heroku-buildpack-claude-code" }
  ]
}
```

### 2. Deploy

```bash
git push heroku main
```

## Configuration

### Environment Variables

Set your Anthropic API key for Claude Code to authenticate:

```bash
heroku config:set ANTHROPIC_API_KEY=your-api-key
```

Or if using Claude via other providers:

```bash
# For Amazon Bedrock
heroku config:set CLAUDE_CODE_USE_BEDROCK=1
heroku config:set AWS_ACCESS_KEY_ID=your-key
heroku config:set AWS_SECRET_ACCESS_KEY=your-secret
heroku config:set AWS_REGION=us-east-1

# For Google Vertex AI
heroku config:set CLAUDE_CODE_USE_VERTEX=1
heroku config:set GOOGLE_APPLICATION_CREDENTIALS=/path/to/credentials.json
```

## Using Claude Code

Once deployed, you can use Claude Code in your dyno:

```bash
# Start a one-off dyno
heroku run bash

# Run Claude Code
claude
```

Or use it programmatically in scripts:

```bash
# Run a single command
heroku run claude -p "Analyze the code in src/"

# Use in a Procfile for automated tasks
# worker: claude -p "Monitor logs and report issues"
```

## Use Cases

- **Automated code review**: Run Claude Code in CI/CD pipelines
- **Log analysis**: Have Claude analyze application logs
- **Code generation**: Generate code as part of your build process
- **Documentation**: Auto-generate documentation during deployment

## Requirements

- Heroku Cedar stack (heroku-20, heroku-22, or heroku-24)
- A Claude subscription (Pro, Max, Teams, or Enterprise) or Anthropic API access

## Caching

The buildpack caches the Claude Code installation for 24 hours to speed up subsequent builds. The cache is automatically invalidated after 24 hours to ensure you get updates.

To force a fresh installation, clear the build cache:

```bash
heroku builds:cache:purge -a your-app-name
```

## Troubleshooting

### Claude Code not found in PATH

Ensure the `.profile.d/claude-code.sh` script is being sourced. Check with:

```bash
heroku run "echo \$PATH"
```

The path should include `.claude-code-bin`.

### Authentication issues

Make sure your `ANTHROPIC_API_KEY` is set correctly:

```bash
heroku config:get ANTHROPIC_API_KEY
```

### Build failures

Check the build logs for detailed error messages:

```bash
heroku builds:info -a your-app-name
```

## License

MIT License - see LICENSE file for details.
