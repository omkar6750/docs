# CometChat SDK & UI Kit Documentation Improvement Guidelines

These guidelines document the patterns, standards, and improvements applied to the JavaScript SDK docs. Use this as a reference when improving documentation for other technologies (Android, iOS, Flutter, React Native, etc.) with AI assistance.

---

## Table of Contents

1. [Quick Reference Blocks (Agent-Friendly)](#1-quick-reference-blocks-agent-friendly)
2. [Available Via Notes (Feature Pages Only)](#2-available-via-notes-feature-pages-only)
3. [Code Examples: Multi-Language Tabs](#3-code-examples-multi-language-tabs)
4. [Mintlify Components Usage](#4-mintlify-components-usage)
5. [Next Steps Navigation](#5-next-steps-navigation)
6. [Page Structure Template](#6-page-structure-template)
7. [Navigation Organization](#7-navigation-organization)
8. [Integration Guides](#8-integration-guides)
9. [Glossary & Key Concepts](#9-glossary--key-concepts)
10. [Security & Init Warnings](#10-security--init-warnings)
11. [Cross-Linking & Message Structure References](#11-cross-linking--message-structure-references)
12. [What NOT to Do](#12-what-not-to-do)
13. [File-by-File Checklist](#13-file-by-file-checklist)
14. [Prompt Template for AI Assistants](#14-prompt-template-for-ai-assistants)

---

## 1. Quick Reference Blocks (Agent-Friendly)

Every content page (not redirect-only pages) should have a Quick Reference block at the very top, immediately after the frontmatter. This is the single most impactful change for AI agent usability.

**Why:** AI agents parsing docs need a fast, copy-paste-ready summary. Developers scanning docs want the TL;DR.

**Pattern:**

```mdx
---
title: "Page Title"
description: "Short description"
---

{/* TL;DR for Agents and Quick Reference */}
<Info>
**Quick Reference for AI Agents & Developers**

```language
// Most common usage — copy-paste ready
const result = await SDK.doThing(param1, param2);

// Second most common pattern
const config = new SDK.Builder()
  .setOption(value)
  .build();

// Key constants or enums if relevant
SDK.TYPE.OPTION_A | OPTION_B
```
</Info>
```

**Rules:**
- Keep it to 5-15 lines of code maximum
- Show the most common use cases only
- Use real method names and constants — no pseudocode
- Include comments explaining what each snippet does
- Use `await` style for brevity (async/await is most readable)
- Skip error handling in the quick reference (full examples come later)
- For overview/hub pages, list links instead of code:

```mdx
<Info>
**Quick Reference for AI Agents & Developers**

Choose your path:
- **Feature A** → [link](/path) - Short description
- **Feature B** → [link](/path) - Short description
</Info>
```

**Skip Quick Reference for:**
- Redirect-only pages (pages that just redirect to another URL)
- Changelog pages
- Pure navigation/index pages with no content

---

## 2. Available Via Notes (Feature Pages Only)

Add an "Available via" note on **feature pages only** — pages that document a user-facing capability.

**What qualifies as a feature page:**
- Messaging (send, receive, edit, delete, reactions, threads, etc.)
- Calling (ringing, call session)
- User management (presence, block, retrieve)
- Group management (create, join, leave, members, etc.)
- Conversations (retrieve, delete, tag)
- Receipts, typing indicators, mentions
- AI features (moderation, agents, copilot)
- Recording, call logs

**What does NOT get "Available via":**
- Setup/installation pages (SDK setup, Calls SDK setup)
- Configuration pages (WebSocket management, session timeout)
- Styling/customization pages (Custom CSS, video view customization)
- Implementation approach pages (standalone calling, presenter mode)
- Reference pages (all listeners, message structure, key concepts)
- Guide pages (integration guides, migration guides)
- Overview/hub pages that just link to sub-pages
- Changelog

**Pattern:**

```mdx
<Note>
**Available via:** SDK | [REST API](https://api-explorer.cometchat.com) | [UI Kits](/ui-kit/react/overview)
</Note>
```

Or simpler when links aren't needed:

```mdx
<Note>
**Availability**: SDK, API, UI Kits
</Note>
```

**Common combinations:**
| Feature Type | Available Via |
|---|---|
| Messaging features | SDK \| REST API \| UI Kits |
| Calling features | SDK \| UI Kits |
| User/Group management | SDK \| REST API \| UI Kits |
| AI features | SDK \| REST API \| UI Kits \| Widget Builder |
| Moderation | SDK \| Dashboard |
| Call logs | SDK \| REST API \| Dashboard |
| Flag/report | SDK \| REST API \| Dashboard |
| Advanced filtering | SDK \| REST API |

**Placement:** Right after the introductory sentence, before the first section heading.

---

## 3. Code Examples: Multi-Language Tabs

Every code example should provide multiple language variants in tabs. The exact tabs depend on the technology.

**For JavaScript SDK:**

```mdx
<Tabs>
<Tab title="JavaScript">
```javascript
CometChat.sendMessage(textMessage).then(
  (message) => console.log("Sent:", message),
  (error) => console.log("Error:", error)
);
```
</Tab>
<Tab title="TypeScript">
```typescript
CometChat.sendMessage(textMessage).then(
  (message: CometChat.TextMessage) => console.log("Sent:", message),
  (error: CometChat.CometChatException) => console.log("Error:", error)
);
```
</Tab>
<Tab title="Async/Await">
```javascript
try {
  const message = await CometChat.sendMessage(textMessage);
  console.log("Sent:", message);
} catch (error) {
  console.log("Error:", error);
}
```
</Tab>
</Tabs>
```

**For other SDKs, adapt accordingly:**
- Android: Java | Kotlin
- iOS: Swift | Objective-C (if supported)
- Flutter: Dart
- React Native: JavaScript | TypeScript

**Rules:**
- Every major code block should have tabs — don't leave single-language examples
- TypeScript tabs should add proper type annotations, not just rename the file
- Async/Await tab should show try/catch pattern
- Simple one-liners (e.g., `CometChat.disconnect()`) can skip tabs — use a single block
- Keep code copy-paste ready: include variable declarations, imports where needed
- Use realistic placeholder values: `"user_uid"`, `"group_guid"`, `"YOUR_APP_ID"`

---

## 4. Mintlify Components Usage

Use these Mintlify components consistently across all pages:

### Steps — For sequential procedures

```mdx
<Steps>
  <Step title="Install the SDK">
    ```bash
    npm install @cometchat/chat-sdk-javascript
    ```
  </Step>
  <Step title="Initialize">
    ```javascript
    await CometChat.init(appID, appSettings);
    ```
  </Step>
</Steps>
```

**Use for:** Setup flows, multi-step procedures, getting started guides.

### Tabs — For code variants and alternative approaches

```mdx
<Tabs>
  <Tab title="npm">...</Tab>
  <Tab title="yarn">...</Tab>
</Tabs>
```

**Use for:** Language variants, package manager options, platform-specific code, alternative approaches (e.g., "To User" vs "To Group").

### CardGroup + Card — For navigation and next steps

```mdx
<CardGroup cols={2}>
  <Card title="Send Messages" icon="paper-plane" href="/sdk/javascript/send-message">
    Send text, media, and custom messages
  </Card>
</CardGroup>
```

**Use for:** Next Steps sections, feature overviews, choosing between options.

### AccordionGroup + Accordion — For best practices and supplementary info

```mdx
<AccordionGroup>
  <Accordion title="Best Practice Name">
    Explanation of the best practice.
  </Accordion>
</AccordionGroup>
```

**Use for:** Best practices, FAQs, troubleshooting tips, edge cases.

### Note, Warning, Info — For callouts

```mdx
<Note>Informational callout — general tips and context.</Note>
<Warning>Critical warning — data loss, security, breaking changes.</Warning>
<Info>Highlighted info — quick references, availability, requirements.</Info>
```

**Use for:**
- `<Note>` — Prerequisites, tips, general information
- `<Warning>` — Destructive operations, security concerns, common mistakes
- `<Info>` — Quick references, feature requirements, plan restrictions

### Frame — For images/screenshots

```mdx
<Frame>
  <img src="/images/screenshot.png" />
</Frame>
```

---

## 5. Next Steps Navigation

Every content page should end with a "Next Steps" section using CardGroup. This creates a natural journey through the docs.

**Pattern:**

```mdx
---

## Next Steps

<CardGroup cols={2}>
  <Card title="Related Feature" icon="icon-name" href="/path/to/page">
    One-line description of what they'll learn
  </Card>
  <Card title="Another Feature" icon="icon-name" href="/path/to/page">
    One-line description
  </Card>
</CardGroup>
```

**Rules:**
- Always use `cols={2}` for consistency
- Include 2-4 cards (not more)
- Link to logically next topics (what would the developer need after this?)
- Use descriptive FontAwesome icon names
- Keep descriptions to one short sentence

**Logical next step patterns:**
| Current Page | Next Steps |
|---|---|
| Send Message | Receive Messages, Edit Message, Message Types |
| Receive Message | Delivery Receipts, Typing Indicators |
| Create Group | Join Group, Add Members |
| Ringing (Call) | Call Session, Call Logs |
| Setup SDK | Authentication, Send First Message |

---

## 6. Page Structure Template

Every feature page should follow this structure:

```
1. Frontmatter (title, sidebarTitle, description)
2. Quick Reference block (Info component with code)
3. Introductory sentence (1-2 lines, what this feature does)
4. Available Via note (feature pages only)
5. Main content sections with code examples in tabs
6. Parameter tables after code examples
7. Best Practices (AccordionGroup) — if applicable
8. Next Steps (CardGroup)
```

**Frontmatter template:**

```yaml
---
title: "Human-Readable Title"
sidebarTitle: "Short Sidebar Name"  # Optional, only if title is too long
description: "One sentence describing what this page covers"
---
```

---

## 7. Navigation Organization

The sidebar navigation in `docs.json` should mirror the CometChat Dashboard feature structure. Group features logically:

```
SDK v4
├── Overview
├── Setup
├── Key Concepts
├── Authentication
│   └── Login, Logout, Auth Tokens
├── Messaging
│   ├── Overview
│   ├── Send Message
│   ├── Receive Message
│   ├── Edit / Delete
│   ├── Threads, Reactions, Mentions
│   └── Advanced (filtering, interactive, transient)
├── Users
│   ├── Overview
│   ├── Presence, Block, Retrieve
│   └── Management
├── Groups
│   ├── Overview
│   ├── Create, Join, Leave, Delete
│   ├── Members, Scope, Ownership
│   └── Retrieve
├── Conversations
├── Calling
│   ├── Overview, Setup
│   ├── Ringing, Call Session, Standalone
│   ├── Recording, Logs, Timeout
│   └── Customization (CSS, Video, Virtual BG, Presenter)
├── AI Features
├── Advanced
├── Integration Guides
├── Framework Guides
└── Resources (Changelog, Rate Limits, Migration)
```

---

## 8. Integration Guides

Create step-by-step integration guides for common scenarios. These are separate from feature docs — they walk through a complete implementation from zero.

**Guides to create:**
- Chat Only (text + media + groups)
- Calls Only (standalone video/audio)
- Chat + Calls (full communication)
- Moderation (content filtering setup)
- Notifications (push alerts)

**Guide structure:**

```
1. What you'll build (outcome)
2. Prerequisites (accounts, keys, dependencies)
3. Step-by-step implementation (Steps component)
4. Complete working code at the end
5. Next steps / what to add
```

**Rules:**
- Every step must have copy-paste ready code
- Include expected output or what the developer should see
- Link back to detailed feature docs for customization
- Keep guides focused — one scenario per guide

---

## 9. Glossary & Key Concepts

The Key Concepts page should include a glossary table defining all CometChat-specific terms.

**Pattern:**

```mdx
## Glossary

| Term | Definition |
|------|-----------|
| UID | Unique User Identifier — alphanumeric string you assign to each user |
| GUID | Group Unique Identifier — alphanumeric string you assign to each group |
| Auth Key | Development-only credential for quick testing. Never use in production |
| Auth Token | Secure, per-user token generated via REST API. Use in production |
| Receiver Type | Specifies if a message target is a `user` or `group` |
| Scope | Group member role: `admin`, `moderator`, or `participant` |
| Listener | Callback handler for real-time events (messages, presence, calls) |
| Conversation | A chat thread between two users or within a group |
| Metadata | Custom JSON data attached to users, groups, or messages |
| Tags | String labels for categorizing users, groups, or conversations |
```

Include 10-20 terms. Define acronyms. Link to relevant pages where the concept is explained in detail.

---

## 10. Security & Init Warnings

Add explicit warnings for common mistakes on relevant pages.

**Init warning (on overview and setup pages):**

```mdx
<Warning>
You must call `CometChat.init()` before using any other SDK method. Calling SDK methods before initialization will throw errors.
</Warning>
```

**Auth Key warning (on authentication pages):**

```mdx
<Warning>
**Auth Key** is for development/testing only. In production, generate **Auth Tokens** on your server using the REST API and pass them to the client. Never expose Auth Keys in production client code.
</Warning>
```

**SSR/Framework note (on overview page):**

```mdx
<Note>
**Server-Side Rendering (SSR):** CometChat SDK requires browser APIs (`window`, `WebSocket`). For Next.js, Nuxt, or other SSR frameworks, initialize the SDK only on the client side using dynamic imports or `useEffect`. See the [Framework Integration](#framework-guides) section.
</Note>
```

---

## 11. Cross-Linking & Message Structure References

Link related concepts together. When a page references a concept explained elsewhere, add an inline link.

**Examples:**
- On Send Message page: "For a deeper understanding of how messages are structured, see [Message Structure & Hierarchy](/sdk/javascript/message-structure-and-hierarchy)."
- On Receive Message page: "To understand the different message categories and types, see [Message Structure & Hierarchy](/sdk/javascript/message-structure-and-hierarchy)."
- On any page using listeners: "Remember to [remove listeners](/sdk/javascript/all-real-time-listeners) when they're no longer needed."

---

## 12. What NOT to Do

These are lessons learned from the JavaScript SDK improvement process. Avoid these mistakes:

1. **Do NOT remove existing prose or explanatory text.** Even if it seems verbose, developers rely on explanations. Only add — never subtract content.

2. **Do NOT remove code examples.** Every code snippet exists for a reason. Add more variants (TypeScript, async/await) but never remove existing ones.

3. **Do NOT remove mermaid diagrams.** Visual aids help developers understand flows.

4. **Do NOT remove framework-specific guides.** Angular, React, Vue guides are valuable even if they seem redundant.

5. **Do NOT minimize or condense docs.** The goal is comprehensive, not concise. More detail is better than less.

6. **Do NOT add "Available via" to non-feature pages.** Setup guides, configuration pages, styling guides, and reference pages should not have availability notes.

7. **Do NOT change section headings** that developers may have bookmarked or that other pages link to.

8. **Do NOT restructure content within pages** unless explicitly asked. Navigation reorganization (sidebar order) is fine; content reorganization within pages is risky.

---

## 13. File-by-File Checklist

Use this checklist when improving each documentation file:

```
[ ] Frontmatter has title, description (and sidebarTitle if needed)
[ ] Quick Reference block present at top (Info component with code)
[ ] "Available via" note present (ONLY if this is a feature page)
[ ] Introductory sentence explains what the feature does
[ ] All code examples have language tabs (JS/TS/Async or platform equivalents)
[ ] Parameter tables follow code examples where applicable
[ ] Mintlify components used appropriately (Steps, Tabs, Cards, Accordions, Notes, Warnings)
[ ] Next Steps section at bottom with CardGroup (2-4 relevant links)
[ ] Cross-links to related pages where concepts are referenced
[ ] Security warnings where applicable (init, auth keys, destructive operations)
[ ] No content has been removed or minimized
[ ] Code is copy-paste ready with realistic placeholders
[ ] Page reads naturally from top to bottom — journey feels logical
```

---

## 14. Prompt Template for AI Assistants

When asking an AI assistant (Kiro, Copilot, etc.) to improve docs, use this prompt structure:

```
Improve the [TECHNOLOGY] SDK documentation files following these guidelines:

1. Add a Quick Reference block at the top of every content page using <Info> component
   with copy-paste ready code snippets (5-15 lines, most common use cases)

2. Add "Available via: SDK | REST API | UI Kits" notes on FEATURE PAGES ONLY
   (not setup, config, styling, or reference pages)

3. Ensure all code examples have language tabs:
   - For JavaScript: JavaScript | TypeScript | Async/Await
   - For [PLATFORM]: [LANGUAGE_A] | [LANGUAGE_B]

4. Add Next Steps navigation at the bottom of every page using <CardGroup cols={2}>
   with 2-4 cards linking to logically next topics

5. Use Mintlify components: <Steps>, <Tabs>, <CardGroup>, <Card>,
   <AccordionGroup>, <Accordion>, <Note>, <Warning>, <Info>

6. CRITICAL: Do NOT remove any existing content, code examples, diagrams,
   or explanatory text. Only ADD improvements.

7. Ensure code is copy-paste ready and technically correct

8. Add cross-links to related pages where concepts are referenced

Reference: sdk/documentation-improvement-guidelines.md
```

---

## Summary of Changes Made to JavaScript SDK Docs

For reference, here is what was done to the JavaScript SDK docs:

1. **Quick Reference blocks** added to all 60 content files
2. **Multi-language tabs** (JavaScript, TypeScript, Async/Await) added to all code examples
3. **Next Steps navigation** (CardGroup) added to bottom of every page
4. **"Available via" notes** added to all feature pages (messaging, calling, users, groups, conversations, AI)
5. **Integration Guides** created: 6 new step-by-step guides (chat-only, calls-only, chat+calls, moderation, notifications, hub page)
6. **Glossary** added to key-concepts.mdx with 15 defined terms
7. **AI Agents page** expanded with event-type handling, complete AIAgentHandler class, cleanup warning
8. **Init warning** added to overview.mdx and setup-sdk.mdx
9. **Message structure cross-links** added to send-message.mdx and receive-message.mdx
10. **SSR/framework note** added to overview.mdx
11. **Navigation reorganized** in docs.json to match CometChat Dashboard feature structure
12. **Mintlify components** used throughout: Steps, Tabs, CardGroup, Card, AccordionGroup, Accordion, Note, Warning, Info, Frame
