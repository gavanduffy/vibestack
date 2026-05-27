Use this tool to create a new Vercel Sandbox — an ephemeral, isolated Linux container that serves as your development environment for the current session. This sandbox provides a secure workspace where you can upload files, install dependencies, run commands, start the Expo Metro bundler, and share QR codes / URLs with users to preview their mobile app via the Expo Go app.

## When to Use This Tool

Use this tool **once per session** when:

1. You begin working on a new user request that requires code execution or file creation
2. No sandbox currently exists for the session
3. The user asks to start a new project, scaffold a mobile app, or test code in a live environment
4. The user requests a fresh or reset environment

## Sandbox Capabilities

After creation, the sandbox allows you to:

- Upload and manage files via `Generate Files`
- Execute shell commands with `Run Command` and `Wait Command`
- Access the Expo Metro bundler through a public URL using `Get Sandbox URL` on port **8081**

Each sandbox mimics a real-world development environment and supports rapid iteration and testing without polluting the local system. The base system is Amazon Linux 2023 with the following additional packages:

```
bind-utils bzip2 findutils git gzip iputils libicu libjpeg libpng ncurses-libs openssl openssl-libs pnpm procps tar unzip which whois zstd
```

You can install additional packages using the `dnf` package manager. You can NEVER use port 8080 as it is reserved for internal applications.

## Expo Go Mobile Workflow

Every project built in this sandbox is an **Expo Go** app. Always expose port **8081** (Expo Metro bundler) when creating the sandbox so the live preview URL can be shared with the user for scanning via the Expo Go app.

Typical workflow:
1. Create sandbox with `ports: [8081]`
2. Scaffold the Expo project with `npx create-expo-app@latest`
3. Start Metro with `npx expo start --tunnel`
4. Call `Get Sandbox URL` on port 8081 to get the scannable URL

## Best Practices

- Create the sandbox at the beginning of the session or when the user initiates a coding task
- Always include port 8081 in the `ports` array for Expo Metro
- Track and reuse the sandbox ID throughout the session
- Do not create a second sandbox unless explicitly instructed
- If the user requests an environment reset, you may create a new sandbox **after confirming their intent**

## Examples of When to Use This Tool

<example>
User: Build me a todo list app I can use on my phone.
Assistant: I'll create a sandbox and scaffold an Expo Go app for you.
*Calls Create Sandbox with ports: [8081]*
</example>

<example>
User: Can we start fresh? I want to rebuild the project from scratch.
Assistant: Got it — I'll create a new sandbox so we can start clean.
*Calls Create Sandbox with ports: [8081]*
</example>

## When NOT to Use This Tool

Skip using this tool when:

1. A sandbox has already been created for the current session
2. You only need to upload files (use Generate Files)
3. You want to execute or wait for a command (use Run Command / Wait Command)
4. You want to preview the application (use Get Sandbox URL)
5. The user hasn't asked to reset the environment

## Summary

Use Create Sandbox to initialize a secure, temporary development environment — but **only once per session**. Always expose port **8081** for the Expo Metro bundler. Treat the sandbox as the core workspace for all follow-up actions unless the user explicitly asks to discard and start anew.
