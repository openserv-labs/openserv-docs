# Agent Starter / OpenServ SDK Tutorial

A starter project to help you get started building AI agents with the [OpenServ SDK](https://github.com/openserv-labs/sdk) - a TypeScript framework that simplifies agent development. Whether you're new to AI development or an experienced developer, this guide will help you get started quickly.

## What You'll Learn

- Setting up your development environment
- Creating a basic AI agent using the OpenServ SDK
- Testing your agent locally with `process()` using OpenAI API
- Deploying your agent to the OpenServ platform

## Prerequisites

- Basic knowledge of JavaScript/TypeScript
- Node.js installed on your computer
- An OpenServ account (create one at [platform.openserv.ai](https://platform.openserv.ai))
- (Optional) An OpenAI API key for local testing

## Getting Started

### 1. Set Up Your Project

First, clone this agent-starter template repository to get a pre-configured project:

```bash
git clone https://github.com/openserv-labs/agent-starter.git
cd agent-starter
npm install
```

### 2. Configure Your Environment

Copy the example environment file and update it with your credentials:

```bash
cp .env.example .env
```

Edit the `.env` file to add:
- `OPENSERV_API_KEY`: Your OpenServ API key (required for platform integration)
- `OPENAI_API_KEY`: Your OpenAI API key (optional, for local testing)
- `PORT`: The port for your agent's server (default: 7378)

### 3. Understand the Project Structure

The agent-starter project has a minimal structure:

```
agent-starter/
├── src/
│   └── index.ts       # Your agent's core logic and server setup
├── .env               # Environment variables
├── package.json       # Project dependencies
└── tsconfig.json      # TypeScript configuration
```

This simple structure keeps everything in one file, making it easy to understand and modify.

## Understanding the Agent Code

Let's examine the `src/index.ts` file to understand how an agent is defined with the SDK and how this works:

### Key Components of the Agent

1. **Agent Creation**: 
   ```typescript
   const agent = new Agent({
     systemPrompt: 'You are an agent that sums two numbers'
   })
   ```
   This creates a new agent with a system prompt that guides its behavior.

2. **Adding Capabilities**: 
   ```typescript
   agent.addCapability({
     name: 'sum',
     description: 'Sums two numbers',
     schema: z.object({
       a: z.number(),
       b: z.number()
     }),
     async run({ args }) {
       return `${args.a} + ${args.b} = ${args.a + args.b}`
     }
   })
   ```
   This defines a capability named `sum` that:
   - Provides a description for the platform to understand when to use it
   - Uses Zod schema for type safety and validation
   - Implements the logic in the `run` function

3. **Starting the Server**:
   ```typescript
   agent.start()
   ```
   This launches an HTTP server that handles requests from the OpenServ platform.

4. **Local Testing with `process()`**:
   ```typescript
   async function main() {
     const sum = await agent.process({
       messages: [
         {
           role: 'user',
           content: 'add 13 and 29'
         }
       ]
     })
   
     console.log('Sum:', sum.choices[0].message.content)
   }
   ```
   This demonstrates how to test your agent locally without deploying it to the platform.

## Testing Locally with `process()`

The `process()` method is a SDK feature that allows you to test your agent locally before deploying it to the OpenServ platform. This is especially useful during development to verify your agent works as expected.

### How `process()` Works

When you call `process()`:

1. The SDK sends the user message to a LLM Large Language Model (using your OpenAI API key)
2. The AI model determines if your agent's capabilities should be used
3. If needed, it invokes your capabilities with the appropriate arguments
4. It returns the response to you for testing

### Testing Complex Inputs and Edge Cases

You can extend the local testing in `main()` to try different inputs:

```typescript
async function main() {
  // Test case 1: Simple addition
  const test1 = await agent.process({
    messages: [{ role: 'user', content: 'add 13 and 29' }]
  })
  console.log('Test 1:', test1.choices[0].message.content)
  
  // Test case 2: Different phrasing
  const test2 = await agent.process({
    messages: [{ role: 'user', content: 'what is the sum of 42 and 58?' }]
  })
  console.log('Test 2:', test2.choices[0].message.content)
  
  // Test case 3: Edge case
  const test3 = await agent.process({
    messages: [{ role: 'user', content: 'add negative five and seven' }]
  })
  console.log('Test 3:', test3.choices[0].message.content)
}
```

## Exposing Your Local Server with Tunneling

During development, OpenServ needs to reach your agent running on your computer. Since your development machine typically doesn't have a public internet address, we'll use a tunneling tool.

### What is Tunneling?

Tunneling creates a temporary secure pathway from the internet to your local development environment, allowing OpenServ to send requests to your agent while you're developing it. Think of it as creating a secure "tunnel" from OpenServ to your local machine.

### Tunneling Options

Choose a tunneling tool:

- [ngrok](https://ngrok.com/) (recommended for beginners)
  - Easy setup with graphical and command-line interfaces
  - Generous free tier with 1 concurrent connection
  - Web interface to inspect requests

- [localtunnel](https://github.com/localtunnel/localtunnel) (open source option)
  - Completely free and open source
  - Simple command-line interface
  - No account required

#### Quick Setup with ngrok

1. [Download and install ngrok](https://ngrok.com/download)
2. Open your terminal and run:

```bash
ngrok http 7378  # Use your actual port number if different
```

3. Look for a line like `Forwarding https://abc123.ngrok-free.app -> http://localhost:7378`
4. Copy the https URL (e.g., `https://abc123.ngrok-free.app`) - you'll need this for the next steps

## Integration with the OpenServ Platform

The `agent.start()` function in your code starts the HTTP server that communicates with the OpenServ platform. When the platform sends a request to your agent:

1. The server receives the request
2. The SDK parses the request and determines which capability to use
3. It executes the capability's `run` function
4. It formats and returns the response to the platform

### Testing on the Platform

To test your agent on the OpenServ platform:

1. **Start your local server**:
   ```bash
   npm run dev
   ```
   or 
  
   ```bash
   npm start
   ```

2. **Expose your server** with a tunneling tool as described in the previous section

3. **Register your agent** on the OpenServ platform:
   - Go to Developer → Add Agent
   - Enter your agent name and capabilities
   - Set the Agent Endpoint to your tunneling tool URL
   - Create a Secret Key and update your `.env` file

4. **Create a project** on the platform:
   - Projects → Create New Project
   - Add your agent to the project
   - Interact with your agent through the platform

## Advanced Capabilities

As you get more comfortable with the SDK, you can leverage more advanced methods and features such as file operations, task management, user interaction via chat and messaging. Check the methods in the [API Reference](https://github.com/openserv-labs/sdk?tab=readme-ov-file#api-reference).

## Production Deployment

When your agent is all set for production, it’s time to get it out there! Just deploy it to a hosting service so that it can be available 24/7 for users to enjoy.

1. **Build your project**:
   ```bash
   npm run build
   ```

2. **Deploy to a hosting service** like (from simplest to most advanced):

    **Serverless** (Beginner-friendly)
      - [Vercel](https://vercel.com/) - Free tier available, easy deployment from GitHub
      - [Netlify Functions](https://www.netlify.com/products/functions/) - Similar to Vercel with a generous free tier
      - [AWS Lambda](https://aws.amazon.com/lambda/) - More complex but very scalable

    **Container-based** (More control)
      - [Render](https://render.com/) - Easy Docker deployment with free tier
      - [Railway](https://railway.app/) - Developer-friendly platform
      - [Fly.io](https://fly.io/) - Global deployment with generous free tier

    **Open source self-hosted** (Maximum freedom)
      - [OpenFaaS](https://www.openfaas.com/) - Functions as a Service for Docker and Kubernetes
      - [Dokku](https://dokku.com/) - Lightweight PaaS you can install on any virtual machine

3. **Update your agent endpoint** on the OpenServ platform with your production endpoint URL

4. **Submit for review** through the Developer dashboard

---

Happy building! We're excited to see what you will create with the OpenServ SDK.
