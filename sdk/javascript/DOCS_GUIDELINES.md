# JavaScript SDK Docs — Structure & Page Template (Guidelines)

These guidelines define the **ideal information architecture (IA)** for the latest CometChat **JavaScript SDK** docs and the **standard structure each page should follow**.

## Scope

- Applies to: `sdk/javascript/*.mdx` (latest docs)
- Excludes legacy: `sdk/javascript/2.0/**`, `sdk/javascript/3.0/**` (do not reference or link from latest docs unless explicitly required)

## Goals (world‑class docs)

- Fast onboarding (developer reaches “send + receive message” quickly).
- Clear separation: **Chat SDK** vs **Calls SDK** vs **AI/Moderation** vs **UI Kits**.
- Every page is skimmable, task-focused, and includes copy‑paste examples (JS + TS).
- Consistent terminology, consistent code patterns, and consistent cross-linking.
- Plan-gated and version-gated features are explicitly labeled.
- Security is baked in (Auth Token for prod, REST keys never in clients).

---

# 1) Ideal Information Architecture (IA)

## 1.1 Start Here

- **Overview**
  - What you can build with the JS SDK
  - Chat SDK vs Calls SDK vs UI Kits (when to choose which)
  - Supported environments and constraints
- **Quickstart (10 minutes)**
  - Install → init → login → send → receive
  - Minimal, runnable “happy path”
- **Install & Setup**
  - NPM + CDN
  - SSR guidance (Next.js/Nuxt)
  - Init patterns
- **Authentication**
  - Auth Key vs Auth Token
  - Session model (`getLoggedinUser`)
  - Logout
- **Key Concepts**
  - UID/GUID constraints
  - Message categories/types
  - Group types/scopes

## 1.2 Chat SDK (Messaging)

- **Messages (Overview)**
  - Links to core tasks below
- **Send messages**
  - Text / Media / Custom / Interactive
  - Metadata, tags, quoted messages, thread replies
- **Receive messages**
  - Real-time listeners
  - Missed/offline messages
- **Message history & pagination**
- **Search messages**
  - Clearly mark plan-gated advanced search
- **Typing indicators**
- **Delivery & read receipts**
  - delivered / read / unread
  - receipt events
- **Edit messages**
- **Delete messages**
- **Threaded messages**
- **Transient messages**
- **Mentions**
- **Reactions**
- **Advanced message filtering**
  - All `MessagesRequestBuilder` filters
  - Plan-gated filters clearly separated and labeled

## 1.3 Conversations (Recent Chats)

- **Retrieve conversations** (build “recent chats” list)
- **Delete conversation (local only)** + link to REST for “delete for everyone”

## 1.4 Users & Presence

- **User management**
  - Create/update users (strong server-side guidance for production)
- **Retrieve users**
  - Filters, pagination, sorting
- **Block / unblock users**
  - Blocked list
- **Presence**
  - Subscription options at init time
  - Presence listeners

## 1.5 Groups

- **Groups overview**
- **Create group**
- **Retrieve groups**
- **Join / leave group**
- **Members**
  - List/add
  - Kick/ban/unban
  - Change scope
- **Update / delete / transfer ownership**
- **Group events**
  - Real-time + action messages in history

## 1.6 AI & Moderation

- **AI Agents**
  - Run lifecycle events (streaming)
  - Persisted agentic messages (via `MessageListener`)
- **AI Moderation**
  - `PENDING → APPROVED/DISAPPROVED`
  - UI patterns for pending/blocked
- **Flag message**
  - reasons → report flow → dashboard review

## 1.7 Calls SDK (Calling)

- **Calling overview**
  - Choose: Ringing vs Call Session vs Standalone
- **Calls SDK setup**
- **Ringing (signaling)**
  - initiate/accept/reject/cancel/busy
  - call listener events
- **Call Session**
  - token → `startSession` → in-call controls → end
- **Standalone calling**
  - REST auth token → call token → `startSession`
- **Calling features** (each as its own page)
  - Recording
  - Virtual background
  - Presenter mode
  - Video view customization
  - Custom CSS
  - Call logs
  - Session timeout
- **Calling troubleshooting**
  - permissions, device selection, cleanup, common errors

## 1.8 Reference

- **Listeners & events** (single canonical list)
  - Recommended registration/removal patterns
  - Event ordering and gotchas
- **Models & fields**
  - `User`, `Group`, `BaseMessage`, `Conversation`, `Call`, etc.
- **Enums & constants**
- **Errors & retries**
  - common exceptions and recommended handling
- **Rate limits**

## 1.9 Releases & Migration

- **Changelog** (link + “what changed” summaries if needed)
- **Upgrade guides** (only for active legacy users; keep clearly separated)

## 1.10 Help

- **FAQ**
- **Troubleshooting**
  - SSR pitfalls, duplicate listeners, offline sync patterns, performance

---

# 2) Standard Structure for Each Page (Template)

Every task page should follow this structure (use these headings in order unless a page is a redirect or purely conceptual):

1. **Summary**
   - 1–2 sentences: what the page enables.
2. **When to use this**
   - Bullet list of common use cases.
3. **Prerequisites**
   - Required SDK(s), init/login state, dashboard toggles, plans, and minimum SDK version.
4. **Minimal example**
   - A short “happy path” snippet that works end-to-end.
   - Include both JavaScript and TypeScript (tabs) when applicable.
5. **Step-by-step**
   - Numbered steps with small snippets.
   - After each step, describe what should happen (expected behavior).
6. **API details**
   - Signature(s)
   - Parameters table: name, type, required, default, notes
   - Return value(s) + side effects
   - Error cases + how to handle
7. **Events & real-time behavior**
   - Which listeners fire, typical ordering, multi-device notes.
   - Always include listener removal/cleanup guidance.
8. **Edge cases & pitfalls**
   - “Gotchas” section that prevents support tickets.
9. **Performance & scale notes**
   - Pagination limits, batching guidance, local caching patterns.
10. **Security notes**
   - What must be server-side, key handling, recommended auth.
11. **Next steps**
   - 3–6 relevant links to adjacent tasks.

---

# 3) Cross-cutting Conventions

## 3.1 Terminology

- Use **Chat SDK** for messaging/users/groups/conversations features.
- Use **Calls SDK** for `CometChatCalls.*`.
- Use **UI Kit** for prebuilt UI components; link out instead of duplicating full UI instructions.

## 3.2 Code sample rules

- Provide **JS + TS** samples (use `<Tabs>` / `<Tab>` consistently).
- Prefer `async/await` in longer examples; keep short examples in promise style if existing docs do.
- Always use consistent placeholders: `APP_ID`, `REGION`, `AUTH_KEY`, `AUTH_TOKEN`, `UID`, `GUID`, `MESSAGE_ID`, `SESSION_ID`.
- Avoid pseudo-APIs in examples. If an API name is uncertain, verify against the SDK or existing docs before publishing.
- For any listener example:
  - Show **add** and **remove** patterns.
  - Emphasize **unique `listenerId`** usage and cleanup.

## 3.3 Plan-gated / version-gated features

- If a feature requires a plan toggle (e.g., “Conversation & Advanced Search”), add a callout near the first mention:
  - What plan/toggle is required
  - Where to enable in Dashboard
- If a feature requires a minimum SDK version, add a short “Available since vX.Y.Z” note.

## 3.4 Security guidance (must be consistent)

- Auth Key in client code is acceptable for **POC/dev**, not production.
- Production guidance: use **Auth Token** (created server-side via REST).
- Never show REST API keys in client code samples.

## 3.5 Linking and page boundaries

- One page = one primary job to be done.
- Cross-link rather than duplicating long sections (e.g., “init once”).
- Keep “overview” pages short: explain concept and link to tasks.

---

# 4) Page Quality Checklist (PR-ready)

Before considering a page “done”:

- The page follows the standard template headings (or is explicitly a redirect).
- There is a minimal working example (JS + TS).
- All API names are consistent across the page and match the SDK/doc canon.
- Any plan/version gating is clearly labeled.
- The page includes at least 3 real pitfalls and how to avoid them.
- There is explicit listener cleanup guidance where listeners are involved.
- Next steps links are present and relevant.

