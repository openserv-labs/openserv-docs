# OpenServ Agent Deployment Guide: From Local Development to Production
In this guide, we'll walk you through deploying with Render.

## Step-by-Step Deployment Guides

### [Render](https://render.com/) Deployment Guide

1. **Create a New Web Service**
   - Go to your Render Dashboard
   - Click **New +** → **Web Service**
   - Connect your git service platform account (GitHub/GitLab/Bitbucket) and select your agent repository

2. **Configure Service Details**
   - Environment: `node`
   - Build Command: `npm install && npm run build`
   - Start Command: `npm run start`
   - Branch: `main` (or your preferred branch)

3. **Set Environment Variables**
   - Add the following variables:
     - `OPENSERV_API_KEY`: Your secret key from the OpenServ agent details page
     - `OPENAI_API_KEY`: Your OpenAI API key (if required)

4. **Deploy and Connect**
   - Click **Deploy Web Service** and wait for the deployment to complete
   - Copy your service/ agent URL (e.g., `https://my-custom-openserv-agent.onrender.com/`)
   - Paste this URL into the Endpoint URL field on the OpenServ agent details page

## Why Host Your OpenServ Agent?

In this context, "deployment" means making your AI agent accessible via the internet so it can communicate with the OpenServ platform. It's the process of taking your code from running locally on your computer to running on a server that's publicly available online.

Deployment involves setting up your agent on a hosting service that provides a stable URL endpoint. This endpoint becomes the communication channel between your agent and the OpenServ infrastructure, allowing your agent to receive requests, process them, and return responses 24/7 without requiring your personal computer to stay online.

Think of it like moving your agent from a private workshop (your local machine) to a public storefront (a hosted server) where it can serve customers (OpenServ users) at any time, even when you're not personally available to manage it.

## Deployment Options

You can deploy your agent to any hosting platform you prefer or even set it up on your own server. Each hosting option comes with its own set of advantages and disadvantages that will match differently with your specific needs as a developer, your technical expertise, and what your production environment requires.

For instance, beginner-friendly platforms offer simplicity but may limit customization, while self-hosting gives you complete control but requires more technical knowledge to maintain. Some services provide generous free tiers ideal for testing, while others offer robust scaling capabilities essential for high-traffic agents. Your choice ultimately depends on factors like your budget, expected traffic, required reliability, and how much time you want to spend on server management versus agent development.

### Beginner-Friendly
These platforms handle most of the deployment complexity for you. They offer simple user interfaces, automated setups, and handle infrastructure management behind the scenes. You typically just connect your GitHub repository, set a few configuration options, and the platform handles the rest.

* [Railway](https://railway.app/)
* [Render](https://render.com/)
* [Vercel](https://vercel.com/)
* [Netlify Functions](https://www.netlify.com/)

### Intermediate
These options require more technical knowledge about concepts like containers and cloud infrastructure. While they still provide managed services, they expose more configuration options and expect users to understand deployment concepts at a deeper level. 

* [Fly.io](https://fly.io/)
* [DigitalOcean App Platform](https://www.digitalocean.com/)
* [Google Cloud Run](https://cloud.google.com/run)

### Advanced (Self-Hosted)
Self-hosted options put you in complete control of your infrastructure. You need to provision your own servers (either physical or virtual), handle all security concerns, manage scaling, and maintain uptime yourself. 

* [OpenFaaS](https://docs.openfaas.com/)
* [Dokku](https://dokku.com/docs/)
* [Coolify](https://coolify.io/)

> **Community Collaboration**: This guide is a contributions of our community member [Mathieu Barber]( https://netfizz.fr/ ) for sharing their knowledge!