# Cursor IDE Setup

- [X] [Use a Thinking Model](#use-a-thinking-model)
- [X] [User Rules](#user-rules)
- [X] [Project Specs File](#project-specs-file)

## Use a Thinking Model

I have found that I get significantly better results when I disable Cursors "Auto-select" Agent model and use a "Thinking" model instead. While it is a bit slower, it yields way more accurate results, especially if you are working with complex code.

This solution works well, as it instructs Cursor to generate ideas and to analyze them **before** generating any solutions. Non-thinking models, which you will likely get when you have `Auto-select` enabled will spit out the first solution they come up with vs. selecting the best one from possible solutions it detected. Enabling this "Thinking" mode also allows you, as a developer, to "see" what the AI Model is considering, which can give you amazing insight into how AI is approaching solving your problem.

### Instructions

Open Model Selection Menu:

![Open Model Selection Menu](docs/img/select-1.png)

Disable `Auto-select` model:

![Disable Auto-select](docs/img/select-2.png)

Enable `Thinking` model, and choose `claude-3.7-sonnet`

![Agent Selection Interface](docs/img/select-3.png)

## User Rules

User rules allow you to provide Global guidance to the Cursor Agent. They apply to all projects and are always included in your model context.

### Update Setting

> Cursor › Settings... › Cursor Settings › Rules › User Rules

Here is a good starter set of User Rules that have worked for most of my projects ( sorted in order of importance ):

<details>
    <summary>➡️️ View: User Rules</summary>

```markdown
- Ask for clarification if feature scope or requirements are unclear.
- Only implement the functionality we’ve discussed; don’t add scaffold or demo code.
- Preserve existing comments; add new ones only to explain non-obvious logic.
- Follow existing naming conventions exactly, including case sensitivity (e.g. CONSTANT_CASE for constants, camelCase for variables, PascalCase for classes).
- Mirror the project’s coding patterns (e.g. named vs. arrow functions, module/export style) wherever possible.
- Adopt the project’s documentation style (JSDoc, docstrings, inline specs) when generating or updating code.
- Respect the project’s indentation, file organization, and lint/formatter configs (ESLint, Prettier, .editorconfig, etc.).
- Ensure all generated code compiles cleanly and passes existing linting/tests.
- Don’t duplicate functionality across files; extend or refactor existing code instead.
- Only introduce new dependencies when absolutely necessary—and after you’ve checked with the dev.
- Avoid changing build or config files (e.g. `package.json`, webpack/Vite/Next configs) unless explicitly asked.
- Follow existing UI patterns and any CSS utility classes; don’t invent new styling conventions.
- Don’t auto-generate tests unless tests are explicitly requested.
- Never leave placeholder text (e.g., TODO, lorem ipsum) in committed code.
- Keep diffs minimal and focused—avoid broad refactors or directory moves without approval.
- When modifying a file, touch only the lines needed for the requested change.
- If a file named `project-specs.md` exists at the project root, always load its contents at startup and treat it as the authoritative source for conventions, folder structure, tech choices, and design decisions.
- When you introduce or modify any convention, dependency, folder structure, or feature in code, update `project-specs.md` if it exists to reflect that change—keeping its prose concise while capturing all relevant information.
```
</details>

**NOTE**: I have noticed that Cursor does not always follow these rules, but most of the time it does. So just be on the lookup for the times Cursor gets ... creative.

## Project Specs File

Cursors AI Agent cannot edit Cursor User Rules, but it can make changes to a local project file. Knowing this, let's create a new file that gives Cursor a bit more context into our project.

In the **User Rules** section above, you might have seen mention of a file named `project-specs.md`. This is a custom file we are creating in the root of our project to help provide the missing context the AI can use to help make better decisions.

When leveraging AI within Cursor, it has limited context about your project, which can make it difficult for AI to make the correct choices. We can assist Cursors AI Agent by providing it additional context.

There are four main sections we can provide in this document that will go a long way towards helping Cursor know a bit more about our project:

1. **Project Description** – A human-readable overview of the project that explains the overall purpose and goals, written to inform an AI assistant.
2. **Features & Technologies** – A bulleted list of the main frameworks, packages, and libraries used. This helps the AI know which tools to prioritize.
3. **Folder Structure** – A summarized list of key folders and their roles in the project, presented as relative paths with descriptions.
4. **Conventions** – Team-specific practices or rules (e.g., coding style, file naming conventions, comment expectations).

<details>
    <summary>➡️️ Sample: project-specs.md</summary>

```markdown
# Project Specs

This file describes the project we are building and its conventions & design decisions. Every coding task must be done in close alignment with this document. The
AI agent should modify this file to always keep it up-to-date with the projects design decisions. Formulations are to be kept as concise as possible while conveying all relevant information.

## Project Description

A travel blog platform powered by Storyblok and AI chat features supported by AWS Lambda functions.

## Features & Technologies

- Next.js
- React
- Tailwind CSS
- Storyblok CMS via @storyblok/react
- AWS Lambda (via Vercel Functions for chat and ingest)
- TypeScript
- ESLint for linting and code quality

## Folder Structure

- `src/app/` → Next.js App Router pages, layouts, and API routes.
- `src/components/` → React components for site sections like Hero, Header, Newsletter, etc.
- `src/lib/` → Shared library code, such as Storyblok initialization.
- `src/utils/` → Utility functions, e.g., content fetching.
- `functions/chat/` → AWS Lambda function handling chat logic.
- `functions/ingest/` → AWS Lambda function for content ingestion, including from Storyblok.

## Conventions

- TypeScript is used across the project for type safety.
- File naming uses PascalCase for components (e.g., `HeroSection.tsx`) and camelCase for utilities.
- Use co-location: components and their styles or subparts live in the same folder unless reused globally.
- Storyblok content is fetched and parsed using `fetchStory.ts` and related utilities.
```
</details>

### ChatGPT Agent

I have created the following ChatGPT Agent you can use to create this file:

[![Cursor Specs Generator](https://img.shields.io/badge/Cursor_Specs_Generator-169BD7.svg?logo=samsclub&logoColor=white&style=for-the-badge "Cursor Specs Generator")]([https://peterschmalfeldt.com](https://chatgpt.com/g/g-68204e69a43881919580f0fed0a2a72a-cursor-specs-generator))
