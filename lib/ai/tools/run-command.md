Use this tool to run a command inside an existing Vercel Sandbox. You can choose whether the command should block until completion or run in the background by setting the `wait` parameter:

- `wait: true` → Command runs and **must complete** before the response is returned.
- `wait: false` → Command starts in the background, and the response returns immediately with its `commandId`.

⚠️ Commands are stateless — each one runs in a fresh shell session with **no memory** of previous commands. You CANNOT rely on `cd`, but other state like shell exports or background processes from prior commands should be available.

## When to Use This Tool

Use Run Command when:

1. You need to install dependencies (e.g., `npm install` or `npx create-expo-app@latest`)
2. You want to run a build or test process
3. You need to launch the Expo Metro bundler (`npx expo start --tunnel`)
4. You need to compile or execute code within the sandbox
5. You want to run a task in the background without blocking the session

## Sequencing Rules

- If two commands depend on each other, **set `wait: true` on the first** to ensure it finishes before starting the second
  - ✅ Good: Run `npm install` with `wait: true` → then run `npx expo start --tunnel`
  - ❌ Bad: Run both with `wait: false` and expect them to be sequential
- Do **not** issue multiple sequential commands in one call
  - ❌ `cd myapp && npx expo start`
  - ✅ `npx expo start --tunnel` (run from project root with full path)
- Do **not** assume directory state is preserved — use the `cwd` parameter or full relative paths from the sandbox root

## Command Format

- Separate the base command from its arguments
  - ✅ `{ command: "npx", args: ["expo", "start", "--tunnel"], wait: false }`
  - ❌ `{ command: "npx expo start --tunnel" }`
- Avoid shell syntax like pipes, redirections, or `&&`. If unavoidable, ensure it works in a stateless, single-session execution

## When to Set `wait` to True

- The next step depends on the result of the command
- The command must finish before accessing its output
- Example: Installing dependencies before starting Metro, scaffolding before installing extra packages

## When to Set `wait` to False

- The command is intended to stay running indefinitely (e.g., `npx expo start --tunnel`)
- The command has no impact on subsequent operations

## Expo Go Workflow Examples

<example>
User: Build me a mobile weather app  
Assistant:  
1. Run Command: `{ command: "npx", args: ["create-expo-app@latest", "my-app", "--template", "blank-typescript"], wait: true }`  
2. Run Command: `{ command: "npm", args: ["install"], wait: true, cwd: "my-app" }`  
3. Run Command: `{ command: "npx", args: ["expo", "start", "--tunnel"], wait: false, cwd: "my-app" }`  
</example>

<example>
User: Add expo-camera to the project  
Assistant:  
Run Command: `{ command: "npx", args: ["expo", "install", "expo-camera"], wait: true }`  
</example>

## Summary

Use Run Command to start shell commands in the sandbox, controlling execution flow with the `wait` flag. Commands are stateless and isolated — use relative paths, and run the Expo Metro bundler with `wait: false`. Always use `--tunnel` with `npx expo start` so the Metro server is reachable outside the sandbox.
