Use this tool to generate and upload code files into an existing Vercel Sandbox. It leverages an LLM to create file contents based on the current conversation context and user intent, then writes them directly into the sandbox file system.

The generated files should be considered correct on first iteration and suitable for immediate use in the sandbox environment. This tool is essential for scaffolding Expo Go apps, adding new screens/components, writing configuration files, or fixing missing files.

All file paths must be relative to the sandbox root (e.g., `app/(tabs)/index.tsx`, `components/Button.tsx`, `package.json`).

## When to Use This Tool

Use Generate Files when:

1. You need to create one or more new files as part of a feature, scaffold, or fix
2. The user requests code that implies file creation (e.g., new screens, navigation, components, services)
3. You need to bootstrap a new Expo app structure inside a sandbox
4. You’re completing a multi-step task that involves generating or updating source code
5. A prior command failed due to a missing file, and you need to supply it

## Expo Go File Generation Guidelines

- Every file must be complete, valid, and runnable where applicable
- Use **Expo Router** (file-based routing under `app/`) for navigation
- Use `StyleSheet.create` or `NativeWind` for styling — never web-only CSS
- Use `expo-*` packages wherever possible (e.g. `expo-image` instead of `<Image>`)
- Wrap root layouts in `<SafeAreaProvider>` and screens in `<SafeAreaView>`
- File contents must reflect the user's intent and the overall session context
- File paths must follow Expo Router conventions (`app/`, `components/`, `hooks/`, `assets/`)
- Generated files should assume compatibility with other existing files in the sandbox

## Best Practices

- Avoid redundant file generation if the file already exists and is unchanged
- Use conventional Expo file/folder structures
- If replacing an existing file, ensure the update fully satisfies the user's request

## Examples of When to Use This Tool

<example>
User: Add a profile screen with an avatar and a settings button
Assistant: I'll generate the profile screen and wire it into the Expo Router navigation.
*Uses Generate Files to create:*
- `app/profile.tsx` — Profile screen with avatar and settings button
- Updated `app/(tabs)/_layout.tsx` to include the profile tab
</example>

<example>
User: Add a custom `Card` component for displaying items
Assistant: I'll generate the Card component.
*Uses Generate Files to create:*
- `components/Card.tsx` with props and StyleSheet
</example>

## When NOT to Use This Tool

Avoid using this tool when:

1. You only need to execute code or install packages (use Run Command instead)
2. You're waiting for a command to finish (use Wait Command)
3. You want to preview a running server or UI (use Get Sandbox URL)
4. You haven't created a sandbox yet (use Create Sandbox first)

## Output Behavior

After generation, the tool will return a list of the files created, including their paths and contents. These can then be inspected, referenced, or used in subsequent commands.

## Summary

Use Generate Files to programmatically create or update files in your Vercel Sandbox. It enables fast iteration, contextual coding, and dynamic file management — all driven by user intent and conversation context. Always follow Expo Go and Expo Router conventions.
