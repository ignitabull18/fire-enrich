# Coolify Deployment Guide

This guide will walk you through deploying your Fire-Enrich application to Coolify.

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

### 6. Configure Environment Variables

You will need to add the following environment variables to your Coolify project.

#### Required at Startup:

- `OPENAI_API_KEY`: Your API key for OpenAI services. This is essential for the application's core functionality.
- `FIRECRAWL_API_KEY`: Your API key for Firecrawl services. This is also essential for the application's core functionality.

#### Optional:

- `ANTHROPIC_API_KEY`: Your API key for Anthropic services. This is only required if you intend to use Anthropic models.
- `UPSTASH_REDIS_REST_URL`: The REST URL for your Upstash Redis instance. This is used for rate limiting in production.
- `FIRE_ENRICH_UNLIMITED`: Set to `true` to enable unlimited mode.
- `FIRESTARTER_DISABLE_CREATION_DASHBOARD`: Set to `true` to disable the creation dashboard.

### 7. Deploy

Once you've configured everything, click the "Deploy" button. Coolify will pull the code from your repository, build the Docker image, and deploy your application. 