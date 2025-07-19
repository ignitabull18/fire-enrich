# Coolify Deployment Guide

This guide will walk you through deploying your Fire-Enrich application to Coolify.

## Prerequisites

- A Coolify instance (self-hosted or cloud)
- GitHub account with this repository forked
- Required API keys (see Environment Variables section)

## Step-by-Step Deployment

### 1. Fork this Repository

Start by forking this repository to your own GitHub account.

### 2. Connect Your GitHub Account to Coolify

If you haven't already, connect your GitHub account to Coolify. You can do this in the "Settings" or "Source Control" section of your Coolify dashboard.

### 3. Create a New Application

- Navigate to your Coolify dashboard and create a new application.
- Select "Git Repository" as the deployment source.

### 4. Select the Repository

- Choose the forked `fire-enrich` repository from the list.
- Select the `main` branch for deployment.

### 5. Configure Build and Deployment

- **Build Pack:** Coolify will automatically detect the `Dockerfile` and use the Docker build pack. If not, manually select "Docker" or "Dockerfile".
- **Port:** The application runs on port `3000`. Ensure this is correctly configured in your Coolify settings.
- **Health Check:** The application includes a built-in health check at `/api/check-env` that runs every 30 seconds.

## Environment Variables Configuration

You will need to add the following environment variables to your Coolify project.

### Required Variables (Essential for Startup)

#### Core API Keys
- **`OPENAI_API_KEY`**: Your API key for OpenAI services
  - Get from: https://platform.openai.com/api-keys
  - Essential for AI-powered data extraction and enrichment
  
- **`FIRECRAWL_API_KEY`**: Your API key for Firecrawl services
  - Get from: https://www.firecrawl.dev/app/api-keys
  - Essential for web scraping and content aggregation

### Optional Variables (Production Enhancements)

#### Additional AI Services
- **`ANTHROPIC_API_KEY`**: Your API key for Claude models
  - Get from: https://console.anthropic.com/
  - Only needed if you plan to use Anthropic models

#### Rate Limiting (Highly Recommended for Production)
- **`UPSTASH_REDIS_REST_URL`**: The REST URL for your Upstash Redis instance
  - Get from: https://upstash.com/
  - Used for API rate limiting in production
  - Format: `https://your-redis-id.upstash.io`

- **`UPSTASH_REDIS_REST_TOKEN`**: Your Upstash Redis REST token
  - Get from your Upstash dashboard
  - Required alongside `UPSTASH_REDIS_REST_URL`

#### Feature Configuration
- **`FIRE_ENRICH_UNLIMITED`**: Set to `true` to enable unlimited mode
  - Removes restrictions on rows, columns, and processing limits
  - Automatically enabled in development

- **`FIRESTARTER_DISABLE_CREATION_DASHBOARD`**: Set to `true` to disable the creation dashboard
  - Optional UI customization

- **`NEXT_TELEMETRY_DISABLED`**: Set to `1` to disable Next.js telemetry
  - Recommended for production deployments

### Setting Up Redis Rate Limiting (Recommended)

1. **Create Upstash Redis Database:**
   - Visit https://upstash.com/
   - Create a new Redis database
   - Choose a region close to your Coolify deployment

2. **Get Connection Details:**
   - Copy the "REST URL" (starts with `https://`)
   - Copy the "REST Token" from your database dashboard

3. **Add to Coolify:**
   - Add both `UPSTASH_REDIS_REST_URL` and `UPSTASH_REDIS_REST_TOKEN` as environment variables
   - Without Redis, rate limiting is disabled in production (not recommended for public deployments)

### 6. Deploy

Once you've configured everything, click the "Deploy" button. Coolify will:
1. Pull the code from your repository
2. Build the Docker image using the optimized multi-stage build
3. Deploy your application with health checking enabled

### 7. Verify Deployment

After deployment, verify everything is working:

1. **Health Check**: Visit `https://your-domain.com/api/check-env`
   - Should return JSON with environment status
   - All required API keys should show as `true`

2. **Application Access**: Visit your main domain
   - Should load the Fire Enrich interface
   - Try uploading a small CSV to test functionality

3. **Rate Limiting**: Check Coolify logs for rate limiting messages
   - If Redis is configured, you should see rate limit initialization logs
   - Without Redis, you'll see "Rate limiting disabled" messages

## Troubleshooting

### Common Issues

1. **Build Fails**
   - Check that your fork is up to date with the main repository
   - Verify Node.js version (requires Node 18+)
   - Check Coolify build logs for specific error messages

2. **Application Won't Start**
   - Verify both `OPENAI_API_KEY` and `FIRECRAWL_API_KEY` are set
   - Check the health check endpoint at `/api/check-env`
   - Review Coolify application logs

3. **Rate Limiting Not Working**
   - Verify both Redis environment variables are set correctly
   - Check that your Upstash Redis database is active
   - Rate limiting is automatically disabled in development mode

4. **Slow Performance**
   - Ensure your Coolify instance has adequate resources (2GB+ RAM recommended)
   - Consider setting up Redis for better caching performance
   - Monitor API usage limits on OpenAI and Firecrawl

### Monitoring

- **Health Checks**: Coolify will automatically restart the container if health checks fail
- **Logs**: Monitor application logs through the Coolify dashboard
- **Metrics**: Use the built-in environment status endpoint for monitoring

### Scaling Considerations

For high-volume usage:
- Upgrade to higher-tier API plans for OpenAI and Firecrawl
- Consider horizontal scaling with multiple Coolify instances
- Implement proper Redis clustering for distributed rate limiting
- Monitor resource usage and scale infrastructure accordingly

## Production Recommendations

1. **API Keys**: Store in Coolify's encrypted environment variables
2. **Redis**: Always use Redis for production rate limiting
3. **Monitoring**: Set up alerts for health check failures
4. **Backups**: Coolify handles container restarts, but monitor for data persistence needs
5. **Updates**: Fork updates from the main repository and redeploy as needed

## Support

If you encounter issues:
1. Check the application logs in Coolify
2. Verify all environment variables are correctly set
3. Test the health check endpoint manually
4. Review the project's GitHub issues: https://github.com/mendableai/fire-enrich/issues 