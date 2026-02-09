# CometChat SDK & UI Kit Documentation Improvement Guidelines

These guidelines document the patterns, standards, and improvements applied to the JavaScript SDK docs and React UI Kit docs. Use this as a reference when improving documentation for other technologies (Android, iOS, Flutter, React Native, etc.) with AI assistance.

---

## Table of Contents

1. [Quick Reference Blocks (Agent-Friendly)](#1-quick-reference-blocks-agent-friendly)
2. [Available Via Notes (Feature Pages Only)](#2-available-via-notes-feature-pages-only)
3. [Code Examples: Multi-Language Tabs](#3-code-examples-multi-language-tabs)
4. [Mintlify Components Usage](#4-mintlify-components-usage)
5. [Next Steps Navigation](#5-next-steps-navigation)
6. [Page Structure Templates](#6-page-structure-templates)
7. [Navigation Organization](#7-navigation-organization)
8. [Integration Guides](#8-integration-guides)
9. [Glossary & Key Concepts](#9-glossary--key-concepts)
10. [Security & Init Warnings](#10-security--init-warnings)
11. [Cross-Linking & References](#11-cross-linking--references)
12. [What NOT to Do](#12-what-not-to-do)
13. [File-by-File Checklist](#13-file-by-file-checklist)
14. [Prompt Template for AI Assistants](#14-prompt-template-for-ai-assistants)
15. [UI Kit Component Page Guidelines](#15-ui-kit-component-page-guidelines)
16. [UI Kit File Classification](#16-ui-kit-file-classification)

---

## 1. Quick Reference Blocks (Agent-Friendly)

Every content page (not redirect-only pages) should have a Quick Reference block at the very top, immediately after the frontmatter. This is the single most impactful change for AI agent usability.

**Why:** AI agents parsing docs need a fast, copy-paste-ready summary. Developers scanning docs want the TL;DR.

### SDK Pages Pattern

```mdx
---
title: "Page Title"
description: "Short description"
---

{/* TL;DR for Agents and Quick Reference */}
<Info>
**Quick Reference for AI Agents & Developers**

```javascript
// Most common usage — copy-paste ready
const result = await SDK.doThing(param1, param2);

// Second most common pattern
const config = new SDK.Builder()
  .setOption(value)
  .build();
```
</Info>
```

### UI Kit Component Pages Pattern

For UI Kit component pages, the Quick Reference should show the minimal JSX to render the component plus the most common props:

```mdx
{/* TL;DR for Agents and Quick Reference */}
<Info>
**Quick Reference for AI Agents & Developers**

```tsx
// Minimal usage
<CometChatConversations />

// With common props
<CometChatConversations
  onItemClick={(conversation) => handleSelect(conversation)}
  hideReceipts={false}
  selectionMode={SelectionMode.none}
/>

// Key imports
import { CometChatConversations } from "@cometchat/chat-uikit-react";
```
</Info>
```

### UI Kit Feature Overview Pages Pattern

For feature overview pages (core-features, call-features, ai-features), list the components involved:

```mdx
<Info>
**Quick Reference for AI Agents & Developers**

Key components for this feature:
- **Conversations** → [CometChatConversations](/ui-kit/react/conversations)
- **Message List** → [CometChatMessageList](/ui-kit/react/message-list)
- **Message Composer** → [CometChatMessageComposer](/ui-kit/react/message-composer)
</Info>
```

### UI Kit Reference/Config Pages Pattern

For reference pages (methods, events, localize, sound-manager), show the most-used API calls:

```mdx
<Info>
**Quick Reference for AI Agents & Developers**

```javascript
// Init
CometChatUIKit.init(UIKitSettings);

// Login
CometChatUIKit.login("UID");

// Login with Auth Token (production)
CometChatUIKit.loginWithAuthToken("AUTH_TOKEN");

// Logout
CometChatUIKit.logout();
```
</Info>
```

**Rules (all page types):**

- Keep it to 5-15 lines of code maximum
- Show the most common use cases only
- Use real component names, method names, and prop names — no pseudocode
- Include comments explaining what each snippet does
- For UI Kit: always show the import statement
- Skip error handling in the quick reference (full examples come later)

**Skip Quick Reference for:**

- Redirect-only pages (pages that just redirect to another URL)
- Changelog pages
- Pure navigation/index pages with no content
- Link pages (sample app, figma, changelog links)

---

## 2. Available Via Notes (Feature Pages Only)

Add an "Available via" note on **feature pages only** — pages that document a user-facing capability.

### SDK Feature Pages

**What qualifies as a feature page:**

- Messaging (send, receive, edit, delete, reactions, threads, etc.)
- Calling (ringing, call session)
- User management (presence, block, retrieve)
- Group management (create, join, leave, members, etc.)
- Conversations (retrieve, delete, tag)
- Receipts, typing indicators, mentions
- AI features (moderation, agents, copilot)
- Recording, call logs

### UI Kit Feature & Component Pages

**What qualifies for "Available via" in UI Kit docs:**

- Feature overview pages: `core-features.mdx`, `call-features.mdx`, `ai-features.mdx`, `extensions.mdx`
- Component pages that represent a user-facing feature: `conversations.mdx`, `users.mdx`, `groups.mdx`, `group-members.mdx`, `message-header.mdx`, `message-list.mdx`, `message-composer.mdx`, `thread-header.mdx`, `incoming-call.mdx`, `outgoing-call.mdx`, `call-buttons.mdx`, `call-logs.mdx`, `search.mdx`, `ai-assistant-chat.mdx`

**What does NOT get "Available via" (both SDK and UI Kit):**

- Setup/installation pages (integration guides, getting started)
- Configuration pages (methods, events)
- Styling/customization pages (theme, color-resources, message-bubble-styling)
- Reference pages (methods, events, components-overview, localize, sound-manager)
- Guide pages (all guide-*.mdx, formatter guides)
- Overview/hub pages that just link to sub-pages
- Migration pages (upgrading-from-v5, property-changes)
- Message template page (it's a reference/building-block, not a feature)
- Changelog, sample app links, figma links

**Pattern for UI Kit:**

```mdx
<Note>
**Available via:** [UI Kits](/ui-kit/react/overview) | [SDK](/sdk/javascript/overview) | [REST API](https://api-explorer.cometchat.com)
</Note>
```

**Common combinations for UI Kit pages:**

| Page Type | Available Via |
| --- | --- |
| Conversations, Users, Groups, Group Members | UI Kits \| SDK \| REST API |
| Message List, Message Composer, Message Header | UI Kits \| SDK |
| Thread Header | UI Kits \| SDK |
| Incoming Call, Outgoing Call, Call Buttons | UI Kits \| SDK |
| Call Logs | UI Kits \| SDK \| REST API |
| Search | UI Kits |
| AI Assistant Chat | UI Kits \| SDK |
| AI Features (Copilot, Smart Replies, etc.) | UI Kits \| SDK \| Dashboard |
| Core Features overview | UI Kits \| SDK \| REST API |
| Call Features overview | UI Kits \| SDK |
| Extensions overview | UI Kits \| SDK \| Dashboard |

**Placement:** Right after the introductory paragraph/overview image, before the first `## Usage` or `## Integration` heading.

---

## 3. Code Examples: Multi-Language Tabs

Every code example should provide multiple language variants in tabs. The exact tabs depend on the technology.

### For JavaScript SDK

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

### For React UI Kit

UI Kit code examples use React components (JSX/TSX). The tab convention is:

```mdx
<Tabs>
<Tab title="TypeScript">
```tsx
import React from "react";
import { CometChat } from "@cometchat/chat-sdk-javascript";
import { CometChatConversations } from "@cometchat/chat-uikit-react";

export function ConversationsDemo() {
  const handleItemClick = (conversation: CometChat.Conversation) => {
    console.log("Selected:", conversation);
  };
  return <CometChatConversations onItemClick={handleItemClick} />;
}
```
</Tab>
<Tab title="JavaScript">
```jsx
import React from "react";
import { CometChat } from "@cometchat/chat-sdk-javascript";
import { CometChatConversations } from "@cometchat/chat-uikit-react";

export function ConversationsDemo() {
  const handleItemClick = (conversation) => {
    console.log("Selected:", conversation);
  };
  return <CometChatConversations onItemClick={handleItemClick} />;
}
```
</Tab>
</Tabs>
```

**When CSS is involved, add a CSS tab:**

```mdx
<Tabs>
<Tab title="TypeScript">
```tsx
<CometChatConversations />
```
</Tab>
<Tab title="css">
```css
.cometchat-conversations .cometchat-list-item {
  font-family: "SF Pro";
}
```
</Tab>
</Tabs>
```

**UI Kit Tab Rules:**

- TypeScript is the primary tab (listed first) since UI Kit examples are React/TSX
- JavaScript tab should remove type annotations but keep the same logic
- CSS tab is added when styling customization is shown
- For simple prop demos (one-liner JSX), a single tab is acceptable
- Always include imports in the first code example on a page
- Subsequent examples on the same page can skip imports if they were shown earlier
- Use realistic placeholder values: `"uid"`, `"assistant_uid"`, `"GUID"`

**For other platforms, adapt accordingly:**

- Android: Java | Kotlin
- iOS: Swift | Objective-C (if supported)
- Flutter: Dart
- React Native: JavaScript | TypeScript

---

## 4. Mintlify Components Usage

Use these Mintlify components consistently across all pages:

### Steps — For sequential procedures

```mdx
<Steps>
  <Step title="Install the SDK">
    npm install @cometchat/chat-uikit-react
  </Step>
  <Step title="Initialize">
    CometChatUIKit.init(UIKitSettings);
  </Step>
</Steps>
```

**Use for:** Setup flows, multi-step procedures, getting started guides.

### Tabs — For code variants and alternative approaches

**Use for:** Language variants (TS/JS/CSS), package manager options (npm/yarn), platform-specific code, alternative approaches (e.g., "1:1 chat" vs "Group chat", "To User" vs "To Group").

### CardGroup + Card — For navigation and next steps

```mdx
<CardGroup cols={2}>
  <Card title="Conversations" icon="comments" href="/ui-kit/react/conversations">
    Display and manage conversation lists
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

**Use for:** Best practices, FAQs, troubleshooting tips, edge cases, performance tips.

### Note, Warning, Info — For callouts

```mdx
<Note>Informational callout — general tips and context.</Note>
<Warning>Critical warning — data loss, security, breaking changes.</Warning>
<Info>Highlighted info — quick references, availability, requirements.</Info>
```

**Use for:**

- `<Note>` — Prerequisites, tips, general information, "Available via" notes
- `<Warning>` — Destructive operations, security concerns, common mistakes, required props
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

**Logical next step patterns for UI Kit:**

| Current Page | Next Steps |
| --- | --- |
| Overview | Getting Started (React.js), Components Overview |
| Integration (React.js) | Core Features, Components Overview |
| Conversations | Message List, Users, Groups |
| Message List | Message Composer, Message Header, Message Template |
| Message Composer | Message List, Message Template |
| Message Header | Message List, Thread Header |
| Users | Groups, Group Members |
| Groups | Group Members, Conversations |
| Incoming Call | Outgoing Call, Call Buttons |
| Call Features | Call Buttons, Incoming Call, Call Logs |
| Core Features | Components Overview, Theme |
| Theme | Color Resources, Message Bubble Styling |
| Methods | Events, Components Overview |
| Events | Methods, Components Overview |
| Search | Conversations, Message List |
| AI Assistant Chat | AI Features, Message List |

---

## 6. Page Structure Templates

### SDK Feature Page Structure

```text
1. Frontmatter (title, sidebarTitle, description)
2. Quick Reference block (Info component with code)
3. Introductory sentence (1-2 lines, what this feature does)
4. Available Via note (feature pages only)
5. Main content sections with code examples in tabs
6. Parameter tables after code examples
7. Best Practices (AccordionGroup) — if applicable
8. Next Steps (CardGroup)
```

### UI Kit Component Page Structure

UI Kit component pages follow a well-established pattern. Preserve this structure and enhance it:

```text
1. Frontmatter (title, description)
2. Quick Reference block (Info component with minimal JSX + key props)
3. ## Overview — 1-2 sentences + screenshot
4. Available Via note (for feature-facing components)
5. ## Usage
   a. ### Integration — basic rendering example
   b. ### Actions — callback props (onItemClick, onError, etc.)
   c. ### Filters — RequestBuilder customization
   d. ### Events — CometChat event subscriptions
6. ## Customization
   a. ### Style — CSS overrides with screenshots
   b. ### Functionality — props table
   c. ### Advanced — custom views (itemView, leadingView, trailingView, etc.)
7. ## Next Steps (CardGroup)
```

### UI Kit Feature Overview Page Structure

```text
1. Frontmatter (title, description)
2. Quick Reference block (Info with component links)
3. ## Overview — what this feature category covers
4. Available Via note
5. Feature sections — each with screenshot + component table
6. ## Next Steps (CardGroup)
```

### UI Kit Reference Page Structure

```text
1. Frontmatter (title, description)
2. Quick Reference block (Info with key method calls)
3. ## Overview — what this reference covers
4. Main content (method docs, event tables, etc.)
5. ## Next Steps (CardGroup)
```

### UI Kit Integration/Getting Started Page Structure

```text
1. Frontmatter (title, sidebarTitle, description)
2. Quick Reference block (Info with install + init + login snippet)
3. Intro paragraph
4. ## Prerequisites
5. ## Step-by-step setup (using Steps component or ### Step N headings)
6. ## Choose a Chat Experience (with CardGroup options)
7. ## Next Steps (CardGroup)
```

**Frontmatter template:**

```yaml
---
title: "Human-Readable Title"
sidebarTitle: "Short Sidebar Name"  # Optional, only if title is too long for sidebar
description: "One sentence describing what this page covers"
---
```

---

## 7. Navigation Organization

### SDK Navigation

The sidebar navigation in `docs.json` should mirror the CometChat Dashboard feature structure:

```text
SDK v4
├── Overview
├── Setup & Authentication
├── Core Features (Messaging, Receipts, Typing, Presence, etc.)
├── Users
├── Groups
├── Message Management
├── Calling
├── AI Features
├── Advanced
├── Integration Guides
├── Resources
└── UI Kits (links to UI Kit docs)
```

### UI Kit Navigation

The UI Kit sidebar should follow this structure:

```text
React UI Kit v6
├── Overview
├── Getting Started
│   ├── React.js (Integration → Conversation → 1:1 Chat → Tab-Based)
│   ├── Next.js (Integration → Conversation → 1:1 Chat → Tab-Based)
│   ├── React Router (Integration → Conversation → 1:1 Chat → Tab-Based)
│   └── Astro (Integration → Conversation → 1:1 Chat → Tab-Based)
├── Features
│   ├── Chat (Core, Extensions, AI)
│   └── Call
├── Theming
│   ├── Overview (theme.mdx)
│   ├── Color Resources
│   ├── Message Bubble Styling
│   ├── Localize
│   └── Sound Manager
├── Components
│   ├── Overview
│   ├── Conversations, Users, Groups, Group Members
│   ├── Message Header, Message List, Message Composer
│   ├── Message Template, Thread Header
│   ├── Incoming Call, Outgoing Call, Call Buttons, Call Logs
│   ├── Search
│   └── AI Assistant Chat
├── Reference
│   ├── Methods
│   └── Events
├── Guides
│   ├── Overview
│   ├── Feature guides (threaded messages, block user, etc.)
│   └── Formatter guides (text, mentions, URL, shortcut)
├── Migration Guide
│   ├── Upgrading from v5
│   └── Property Changes
├── Sample App (link)
├── Changelog (link)
└── Figma (link)
```

---

## 8. Integration Guides

### SDK Integration Guides

Create step-by-step integration guides for common scenarios:

- Chat Only (text + media + groups)
- Calls Only (standalone video/audio)
- Chat + Calls (full communication)
- Moderation (content filtering setup)
- Notifications (push alerts)

### UI Kit Guides

The UI Kit already has a robust set of guides. When adding new ones, follow the existing pattern:

- Each guide focuses on one specific task/feature
- Includes complete working code
- Links back to component docs for customization
- Uses the guide-overview.mdx as the index page

**Guide structure:**

```text
1. What you'll build (outcome)
2. Prerequisites (accounts, keys, dependencies)
3. Step-by-step implementation (Steps component or numbered sections)
4. Complete working code at the end
5. Next steps / what to add
```

---

## 9. Glossary & Key Concepts

### SDK Glossary

Include on the Key Concepts page with 10-20 terms (UID, GUID, Auth Key, Auth Token, etc.).

### UI Kit Glossary

Add UI Kit-specific terms to the components-overview or methods page:

| Term | Definition |
| --- | --- |
| UIKitSettings | Configuration object for initializing the UI Kit (App ID, Region, Auth Key) |
| CometChatUIKit | Main class providing init, login, logout, and message-sending wrapper methods |
| Actions | Callback props on components (onItemClick, onError, etc.) |
| Events | Global event bus for cross-component communication (CometChatMessageEvents, etc.) |
| Filters | RequestBuilder props that customize data fetching (ConversationsRequestBuilder, etc.) |
| Views | Custom render props for replacing parts of a component (itemView, leadingView, etc.) |
| CalendarObject | Configuration class for customizing date/time formatting across components |
| SelectionMode | Enum controlling list selection behavior (none, single, multiple) |
| TextFormatter | Base class for custom text processing (mentions, URLs, shortcuts) |
| ChatConfigurator | Singleton providing access to message templates and data source configuration |

---

## 10. Security & Init Warnings

### Init Warning (SDK and UI Kit)

**SDK:**

```mdx
<Warning>
You must call `CometChat.init()` before using any other SDK method. Calling SDK methods before initialization will throw errors.
</Warning>
```

**UI Kit:**

```mdx
<Warning>
You must call `CometChatUIKit.init(UIKitSettings)` before rendering any UI Kit components or calling any SDK methods. Initialization must complete before login.
</Warning>
```

### Auth Key Warning (both SDK and UI Kit)

```mdx
<Warning>
**Auth Key** is for development/testing only. In production, generate **Auth Tokens** on your server using the REST API and pass them to the client. Never expose Auth Keys in production client code.
</Warning>
```

### SSR/Framework Note (UI Kit specific)

```mdx
<Note>
**Server-Side Rendering (SSR):** CometChat UI Kit requires browser APIs (`window`, `WebSocket`, `document`). For Next.js or other SSR frameworks, ensure components render only on the client side. See the [Next.js Integration](/ui-kit/react/next-js-integration) guide.
</Note>
```

### Calls SDK Dependency (UI Kit specific)

```mdx
<Note>
**Calling features** require installing `@cometchat/calls-sdk-javascript` separately. The UI Kit auto-detects it and enables call UI components. See [Call Features](/ui-kit/react/call-features).
</Note>
```

---

## 11. Cross-Linking & References

Link related concepts together. When a page references a concept explained elsewhere, add an inline link.

### SDK Cross-Links

- On Send Message page: "See [Message Structure & Hierarchy](/sdk/javascript/message-structure-and-hierarchy)."
- On any page using listeners: "Remember to [remove listeners](/sdk/javascript/all-real-time-listeners) when no longer needed."

### UI Kit Cross-Links

- On component pages referencing Actions: "Learn about [Actions](/ui-kit/react/components-overview#actions)."
- On component pages referencing Events: "See [Events](/ui-kit/react/events) for the full event reference."
- On component pages using RequestBuilders: "See [SDK filtering docs](/sdk/javascript/additional-message-filtering) for all builder options."
- On component pages with custom views: "See [Message Template](/ui-kit/react/message-template) for message bubble customization."
- On feature pages: Link each component name to its component page.
- On integration pages: Link to [Methods](/ui-kit/react/methods) for init/login details.
- On theming pages: Link to [Color Resources](/ui-kit/react/theme/color-resources) and [GitHub source](https://github.com/cometchat/cometchat-uikit-react/blob/v6/src/styles/css-variables.css).
- Between related components: Message List → Message Composer, Conversations → Message List, Groups → Group Members.

---

## 12. What NOT to Do

These are lessons learned from the JavaScript SDK and React UI Kit improvement process. Avoid these mistakes:

1. **Do NOT remove existing prose or explanatory text.** Even if it seems verbose, developers rely on explanations. Only add — never subtract content.

2. **Do NOT remove code examples.** Every code snippet exists for a reason. Add more variants (TypeScript, JavaScript, CSS) but never remove existing ones.

3. **Do NOT remove screenshots or images.** Visual aids (Frame components with screenshots) are critical for UI Kit docs — they show what the component looks like.

4. **Do NOT remove framework-specific guides.** React.js, Next.js, React Router, Astro guides are all valuable.

5. **Do NOT minimize or condense docs.** The goal is comprehensive, not concise. More detail is better than less.

6. **Do NOT add "Available via" to non-feature pages.** Setup guides, theming pages, reference pages, guide pages, and migration pages should not have availability notes.

7. **Do NOT change section headings** that developers may have bookmarked or that other pages link to. The UI Kit component pages follow a consistent structure (Overview → Usage → Customization) — preserve it.

8. **Do NOT restructure content within pages** unless explicitly asked. Navigation reorganization (sidebar order) is fine; content reorganization within pages is risky.

9. **Do NOT change the existing component page section order.** The pattern of Overview → Usage (Integration, Actions, Filters, Events) → Customization (Style, Functionality, Advanced) is well-established across all UI Kit component pages.

10. **Do NOT add Async/Await tabs to UI Kit component examples.** UI Kit code is React component code (JSX/TSX) — async/await tabs don't apply to component rendering. Only use TypeScript | JavaScript | CSS tabs.

---

## 13. File-by-File Checklist

Use this checklist when improving each documentation file:

### SDK Files

```text
[ ] Frontmatter has title, description (and sidebarTitle if needed)
[ ] Quick Reference block present at top (Info component with code)
[ ] "Available via" note present (ONLY if this is a feature page)
[ ] Introductory sentence explains what the feature does
[ ] All code examples have language tabs (JS/TS/Async)
[ ] Parameter tables follow code examples where applicable
[ ] Mintlify components used appropriately
[ ] Next Steps section at bottom with CardGroup (2-4 relevant links)
[ ] Cross-links to related pages where concepts are referenced
[ ] Security warnings where applicable
[ ] No content has been removed or minimized
[ ] Code is copy-paste ready with realistic placeholders
```

### UI Kit Files

```text
[ ] Frontmatter has title, description (and sidebarTitle if needed)
[ ] Quick Reference block present at top (Info component with JSX + key props)
[ ] "Available via" note present (ONLY if this is a feature/component page)
[ ] Overview section with introductory sentence + screenshot
[ ] All code examples have language tabs (TypeScript/JavaScript, CSS where applicable)
[ ] Props/functionality table is present and complete
[ ] Actions section documents all callback props
[ ] Events section lists emitted events with subscribe/unsubscribe examples
[ ] Customization section covers Style, Functionality, and Advanced views
[ ] Next Steps section at bottom with CardGroup (2-4 relevant links)
[ ] Cross-links to related component pages
[ ] Security warnings on init/login pages
[ ] No content, screenshots, or code examples have been removed
[ ] Code is copy-paste ready with proper imports
[ ] Page follows the established section order (Overview → Usage → Customization)
```

---

## 14. Prompt Template for AI Assistants

When asking an AI assistant (Kiro, Copilot, etc.) to improve docs, use this prompt structure:

### For SDK Docs

```text
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

### For UI Kit Docs

```text
Improve the React UI Kit v6 documentation files following these guidelines:

1. Add a Quick Reference block at the top of every content page using <Info> component
   with minimal JSX rendering + key props (5-15 lines, copy-paste ready)

2. Add "Available via: UI Kits | SDK | REST API" notes on FEATURE and COMPONENT pages ONLY
   (not setup, theming, reference, guide, or migration pages)

3. Ensure all code examples have language tabs:
   - TypeScript (.tsx) | JavaScript (.jsx)
   - Add CSS tab when styling customization is shown
   - Do NOT add Async/Await tabs (UI Kit code is React component code)

4. Add a description field to frontmatter on every page

5. Add Next Steps navigation at the bottom of every page using <CardGroup cols={2}>
   with 2-4 cards linking to logically next topics

6. Use Mintlify components: <Steps>, <Tabs>, <CardGroup>, <Card>,
   <AccordionGroup>, <Accordion>, <Note>, <Warning>, <Info>, <Frame>

7. CRITICAL: Do NOT remove any existing content, code examples, screenshots,
   or explanatory text. Only ADD improvements.

8. Preserve the existing section structure:
   Overview → Usage (Integration, Actions, Filters, Events) → Customization (Style, Functionality, Advanced)

9. Add cross-links between related component pages

10. Add security warnings on init/login pages (Auth Key for dev only, Auth Token for production)

Reference: ui-kit/react/documentation-improvement-guidelines.md
```

---

## 15. UI Kit Component Page Guidelines

### Component Page Anatomy

Every UI Kit component page (conversations, users, groups, message-list, etc.) follows a consistent structure. When improving these pages, enhance each section without changing the order:

#### 1. Overview Section

- 1-2 sentence description of what the component does
- Screenshot in a `<Frame>` showing the default appearance
- This is where the Quick Reference block goes (before the overview text)

#### 2. Usage → Integration

- Minimal code to render the component
- Usually two tabs: the component file and App.tsx
- Should include proper imports

#### 3. Usage → Actions

- Each callback prop gets its own subsection (numbered: 1. OnItemClick, 2. OnSelect, etc.)
- Code example showing how to override the callback
- Both TypeScript and JavaScript tabs

#### 4. Usage → Filters

- Shows how to pass RequestBuilder props
- Links to SDK docs for full builder options

#### 5. Usage → Events

- Table of events emitted by the component
- Subscribe and unsubscribe code examples

#### 6. Customization → Style

- CSS override examples with screenshots (before/after)
- Both component code and CSS tabs

#### 7. Customization → Functionality

- Props table with Property | Description | Code columns
- This is the comprehensive props reference

#### 8. Customization → Advanced

- Custom view props (itemView, leadingView, trailingView, subtitleView, headerView, etc.)
- Each view gets its own subsection with:
  - Default screenshot
  - Customized screenshot
  - Code example (TypeScript + CSS tabs)

### Adding Missing Tabs

Many UI Kit component pages currently have single-tab examples (only TypeScript or only one file). When improving:

- If a code block shows `.tsx` code with type annotations → add a JavaScript tab without types
- If a code block shows `.jsx` code without types → add a TypeScript tab with types
- If a code block shows CSS → keep it as a separate CSS tab alongside the component tab
- Do NOT convert existing single-tab examples to no-tab code blocks

### Props Table Format

The existing UI Kit docs use this table format for props:

```markdown
| Property | Description | Code |
| --- | --- | --- |
| **Hide Receipts** | Disables message read receipts display. | `hideReceipts={false}` |
| **Selection Mode** | Determines selection mode. | `selectionMode={SelectionMode.multiple}` |
```

Preserve this format. When adding new props, follow the same pattern.

---

## 16. UI Kit File Classification

### Files That Get Full Treatment (Quick Reference + Available Via + Next Steps)

**Component pages (Available via: UI Kits | SDK):**
- `conversations.mdx`, `users.mdx`, `groups.mdx`, `group-members.mdx`
- `message-header.mdx`, `message-list.mdx`, `message-composer.mdx`
- `message-template.mdx` (reference — no "Available via", but gets Quick Reference + Next Steps)
- `thread-header.mdx`
- `incoming-call.mdx`, `outgoing-call.mdx`, `call-buttons.mdx`, `call-logs.mdx`
- `search.mdx`, `ai-assistant-chat.mdx`

**Feature overview pages (Available via varies):**
- `core-features.mdx`, `call-features.mdx`, `ai-features.mdx`, `extensions.mdx`

### Files That Get Quick Reference + Next Steps (No "Available Via")

**Setup/integration pages:**
- `overview.mdx`
- `react-js-integration.mdx`, `next-js-integration.mdx`, `react-router-integration.mdx`, `astro-integration.mdx`
- All `*-conversation.mdx`, `*-one-to-one-chat.mdx`, `*-tab-based-chat.mdx`

**Theming/config pages:**
- `theme.mdx`, `theme/color-resources.mdx`, `theme/message-bubble-styling.mdx`
- `localize.mdx`, `sound-manager.mdx`

**Reference pages:**
- `methods.mdx`, `events.mdx`, `components-overview.mdx`

**Guide pages:**
- `guide-overview.mdx`, `guide-threaded-messages.mdx`, `guide-block-unblock-user.mdx`
- `guide-new-chat.mdx`, `guide-message-privately.mdx`, `guide-search-messages.mdx`
- `guide-call-log-details.mdx`, `guide-group-chat.mdx`
- `custom-text-formatter-guide.mdx`, `mentions-formatter-guide.mdx`
- `url-formatter-guide.mdx`, `shortcut-formatter-guide.mdx`

**Migration pages:**
- `upgrading-from-v5.mdx`, `property-changes.mdx`

### Files That Get Minimal Treatment (Skip or Light Touch)

**Link/redirect pages (skip entirely):**
- `link/changelog.mdx`, `link/figma.mdx`, `link/sample.mdx`

**Moved/deprecated pages (skip entirely):**
- All files in `moved/` directory

---

## Summary of Changes Made

### JavaScript SDK Docs (Completed)

1. **Quick Reference blocks** added to all 60 content files
2. **Multi-language tabs** (JavaScript, TypeScript, Async/Await) added to all code examples
3. **Next Steps navigation** (CardGroup) added to bottom of every page
4. **"Available via" notes** added to all feature pages
5. **Integration Guides** created: 6 new step-by-step guides
6. **Glossary** added to key-concepts.mdx with 15 defined terms
7. **AI Agents page** expanded with event-type handling
8. **Init warning** added to overview.mdx and setup-sdk.mdx
9. **Message structure cross-links** added to send-message.mdx and receive-message.mdx
10. **SSR/framework note** added to overview.mdx
11. **Navigation reorganized** in docs.json to match Dashboard structure
12. **Mintlify components** used throughout

### React UI Kit Docs (In Progress)

Improvements being applied following the same guidelines adapted for UI Kit:

1. **Quick Reference blocks** — adding to all ~57 content files
2. **Description frontmatter** — adding to all pages missing it
3. **"Available via" notes** — adding to feature and component pages only
4. **Next Steps navigation** — adding CardGroup to bottom of every page
5. **Cross-links** — adding between related component pages
6. **Security warnings** — adding to overview and methods pages
7. **Missing language tabs** — ensuring TypeScript + JavaScript tabs on all examples
8. **Preserving existing structure** — not changing section order or removing content
