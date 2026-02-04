---
name: Cometchat
description: Technical documentation & Implementation guides to add In-app Messaging & Voice & Video Calling to your apps and websites in minutes.
metadata:
    mintlify-proj: cometchat
    version: "1.0"
---

## How to Use This Skill (agent playbook)
- **When to use**: Any time the user asks about chat/messaging/calling features, CometChat setup, SDK usage, migration, moderation, push, or compliance.
- **What to ask the user first**: platform (web/React/React Native/iOS/Android/Flutter/backend), target features (chat, calls, notifications, AI/moderation), region (US/EU/IN), auth model (JWT vs API key), and whether they already have AppID/API key.
- **Response pattern**:
  1) Confirm platform and features; restate the goal in one line.
  2) Give the shortest viable path (SDK/UI Kit if possible, REST only when needed).
  3) Provide exact init snippet and required keys/placeholders.
  4) Add one validation step and one fallback/troubleshooting check.
  5) Link to the precise doc section (use https://www.cometchat.com/docs/llms.txt entry when unsure).
- **Guardrails**: Never expose real API keys; remind users to keep API keys server-side. Default to regional endpoints that match their data residency.
- **Keep answers concise**: prioritize checklists and code blocks over prose; include curl/examples only when they help the next step.

## Capabilities

CometChat enables agents to build and manage complete real-time communication systems including one-on-one and group messaging, voice/video calling, AI-powered features, content moderation, and multi-channel notifications. Agents can integrate CometChat through REST APIs, SDKs (JavaScript, React Native, iOS, Android, Flutter, Ionic), pre-built UI Kits, or no-code widgets. The platform supports enterprise-grade security, multi-tenancy, webhooks for event-driven workflows, and extensive customization.

## Skills

### Core Messaging
- **User-to-User Chat**: Send and receive direct messages between individual users with real-time delivery
- **Group Messaging**: Create groups and enable multi-user conversations with group management capabilities
- **Message Types**: Support text, media files, audio messages, and custom message formats
- **Message Delivery & Read Receipts**: Track message delivery status and read indicators
- **Typing Indicators**: Show real-time typing status to other conversation participants
- **Message Search**: Search through message history with filtering capabilities
- **Threaded Conversations**: Create message threads for topic-specific sub-conversations
- **Message Editing & Deletion**: Edit or delete sent messages with history tracking
- **Unread Message Count**: Track unread messages per conversation

### User & Group Management
- **User Creation & Authentication**: Create users and issue auth tokens via REST API or SDKs
- **User Profiles**: Manage user metadata, avatars, presence status, and custom fields
- **User Presence**: Track online/offline status and last active timestamps
- **User Blocking**: Block/unblock users to prevent unwanted communication
- **Group Creation**: Create public, private, and protected groups
- **Group Membership**: Add, remove, and manage group members with role-based permissions
- **Group Ownership Transfer**: Transfer group ownership to other members
- **Member Roles**: Assign admin, moderator, or participant roles with scope management
- **User Search**: Search and discover users in the system
- **Friends List**: Manage friend connections and relationships

### Advanced Messaging Features
- **Mentions (@username)**: Tag specific users in messages for notifications
- **Reactions**: Add emoji reactions to messages for quick feedback
- **Message Pinning**: Pin important messages to the top of conversations
- **Message Saving**: Save messages for later reference
- **Message Translation**: Translate messages to different languages
- **Smart Replies**: AI-powered quick reply suggestions for faster responses
- **Message Shortcuts**: Create custom shortcuts for frequently used messages
- **Voice Transcription**: Convert voice messages to text automatically
- **Rich Media Preview**: Display previews for links, images, and media
- **Collaborative Whiteboard**: Embed interactive whiteboard for visual collaboration
- **Collaborative Document**: Share and edit documents within chat
- **Polls**: Create and conduct polls within conversations
- **Reminders**: Set message reminders for follow-ups

### Voice & Video Calling
- **1-to-1 Calling**: Direct voice and video calls between two users
- **Group Calling**: Conference calls with multiple participants
- **Call Recording**: Record calls for later playback and compliance
- **Screen Sharing**: Share screens during calls for presentations
- **Picture-in-Picture**: Display multiple video feeds simultaneously
- **Virtual Background**: Apply backgrounds during video calls
- **Call Logs**: Track call history with duration and participant details
- **Call Customization**: Customize call UI with branding and layouts
- **Grid & Spotlight Layouts**: Switch between different call view layouts
- **In-Call Messaging**: Send messages during active calls
- **Video Broadcasting**: Stream video to multiple viewers

### AI Features
- **AI Conversation Starter**: Generate AI-powered opening messages to initiate conversations
- **AI Smart Replies**: Provide intelligent reply suggestions based on conversation context
- **AI Conversation Summary**: Automatically summarize long conversations
- **AI Agents**: Deploy custom AI agents for customer support and automation
- **AI Moderation**: Detect and flag inappropriate content automatically
- **AI Image Moderation**: Scan images for policy violations
- **Sentiment Analysis**: Analyze message sentiment for insights

### Notifications
- **Push Notifications**: Send real-time alerts via FCM (Android) and APNs (iOS)
- **Email Notifications**: Send email alerts for unread messages with customizable templates
- **SMS Notifications**: Send SMS alerts for critical messages
- **Notification Preferences**: Allow users to customize notification settings
- **Notification Templates**: Create custom notification message templates
- **Quiet Hours**: Respect user-defined quiet hours for notifications
- **Notification Logs**: Debug and monitor notification delivery status

### Content Moderation
- **Message Moderation**: Flag and review messages for policy violations
- **Profanity Filter**: Automatically detect and filter profane language
- **XSS Filter**: Prevent cross-site scripting attacks in messages
- **Data Masking**: Mask sensitive information in messages
- **User Reporting**: Allow users to report inappropriate users
- **Message Reporting**: Allow users to report inappropriate messages
- **Moderation Dashboard**: Review and approve/reject flagged content
- **Virus & Malware Scanner**: Scan file attachments for threats
- **Image Moderation**: Detect inappropriate images automatically
- **Custom Moderation Rules**: Create custom rules for specific content

### Security & Compliance
- **End-to-End Encryption**: Encrypt messages in transit and at rest
- **Disappearing Messages**: Set messages to auto-delete after specified time
- **Role-Based Access Control**: Manage permissions by user roles
- **HIPAA Compliance**: Meet healthcare industry requirements with BAA
- **GDPR Compliance**: Support data privacy and user rights
- **SOC 2 Type 2**: Enterprise security certification
- **ISO 27001**: Information security management certification
- **PIPEDA Compliance**: Canadian privacy law compliance
- **TLS/SSL Encryption**: Secure data transmission
- **AES 256 Encryption**: Strong encryption at rest

### REST APIs
- **User Management**: Create, update, delete, list, and manage users
- **Authentication**: Issue auth tokens and manage user sessions
- **Message APIs**: Send, receive, update, delete, and search messages
- **Group APIs**: Create, update, delete, and manage groups
- **Member APIs**: Add, remove, and manage group members
- **Conversation APIs**: Retrieve, update, and manage conversations
- **Call APIs**: List and retrieve call information
- **Webhook APIs**: Create and manage webhooks for event notifications
- **Moderation APIs**: Manage moderation rules, keywords, and flagged content
- **Notification APIs**: Configure notification providers and preferences
- **Data Import APIs**: Import users, groups, members, and messages
- **Analytics APIs**: Retrieve usage metrics and message statistics
- **Extension APIs**: Manage and configure extensions

### Webhooks & Events
- **Message Events**: Trigger on message send, edit, delete, and reactions
- **User Events**: Trigger on user login, logout, presence changes
- **Group Events**: Trigger on group creation, member join/leave
- **Call Events**: Trigger on call start, end, and recording completion
- **Moderation Events**: Trigger on content flagging and review
- **Custom Events**: Create custom webhook triggers for business logic
- **Event Filtering**: Filter webhooks by event type and user
- **Webhook Management**: Create, update, delete, and test webhooks
- **Retry Logic**: Automatic retry with exponential backoff
- **Event Delivery**: Guaranteed delivery with logging

### UI Kits & Components
- **React UI Kit**: Pre-built components for React applications
- **React Native UI Kit**: Native components for React Native apps
- **iOS UI Kit**: Swift components for iOS applications
- **Android UI Kit**: Kotlin/Java components for Android apps
- **Flutter UI Kit**: Dart widgets for Flutter applications
- **Angular UI Kit**: Components for Angular applications
- **Vue UI Kit**: Components for Vue.js applications
- **Conversation List**: Display list of active conversations
- **Message List**: Render message history with pagination
- **Message Composer**: Input field with media attachment support
- **Call Components**: Incoming/outgoing call UI components
- **User List**: Display searchable user directory
- **Group Management**: Create and manage groups UI
- **Theming**: Customize colors, fonts, and styling
- **Localization**: Multi-language support with i18n

### Widgets & Builders
- **HTML Widget**: Copy-paste widget for web applications
- **WordPress Plugin**: Direct integration for WordPress sites
- **Shopify App**: Native Shopify integration
- **Squarespace Integration**: Embed in Squarespace sites
- **Wix Integration**: Add to Wix websites
- **Webflow Integration**: Integrate with Webflow projects
- **UI Kit Builder**: Visual builder for customizing chat UI
- **Widget Builder**: No-code widget configuration tool

### Data Management
- **Data Import**: Bulk import users, groups, and messages from legacy systems
- **Live Migration**: Migrate data while system is running
- **Historical Data Import**: Import chat history with timestamps
- **Data Export**: Export user and message data for compliance
- **Conversation Management**: Archive, delete, or reset conversations
- **Message Archival**: Archive old messages for compliance
- **Backup & Restore**: Backup and restore chat data

### Multi-Tenancy
- **App Management**: Create and manage multiple applications
- **Tenant Isolation**: Separate data per tenant/application
- **Usage Tracking**: Monitor usage per tenant
- **Billing Integration**: Track costs per application
- **Team Collaboration**: Manage team members and permissions
- **Settings Management**: Configure app-specific settings

### Extensions & Integrations
- **Giphy Integration**: Search and share GIFs in chat
- **Tenor Integration**: Access Tenor GIF library
- **Stickers**: Built-in sticker packs and custom stickers
- **Intercom Integration**: Connect with Intercom support
- **Chatwoot Integration**: Integrate with Chatwoot helpdesk
- **SendGrid Integration**: Send emails via SendGrid
- **Twilio Integration**: SMS delivery via Twilio
- **Custom Webhooks**: Build custom integrations
- **Link Shorteners**: Bitly and TinyURL integration
- **Thumbnail Generation**: Auto-generate image thumbnails

## Workflows

### Quickstarts (pick one)
- **Web/React**: Install `@cometchat-pro/chat`, init with `CometChat.init(appId, region)`, then `CometChat.login(authToken)`; mount ConversationList + MessageList + Composer from the UI Kit when possible.
- **React Native**: Install `@cometchat-pro/react-native-chat` and platform pods, init with AppID/region, login with auth token, then use UI Kit components or JS SDK methods.
- **iOS (Swift)**: Add via Swift Package Manager, call `CometChat.init(appId:region:)`, then `CometChat.login(uid:authToken:)`; request mic/camera permissions for calling.
- **Android (Kotlin/Java)**: Add Gradle dependency, `CometChat.init(this, appId, region)`, `CometChat.login(uid, authToken)`; ensure required permissions in manifest.
- **Server/Backend**: Use REST APIs with the REST API key (server-side only). Common base: `https://{region}.api.cometchat.io/v3`.

### Setting Up a Chat Application
1. Create an account and obtain AppID and API key from CometChat dashboard
2. Create users via REST API: `POST /v3/users` with user details
3. Issue auth tokens: `POST /v3/auth_tokens` for each user
4. Integrate UI Kit or SDK into your application
5. Initialize CometChat with AppID and auth token
6. Configure webhooks for real-time event handling
7. Set up push notifications with FCM/APNs credentials
8. Enable desired extensions (moderation, smart replies, etc.)

### Implementing One-to-One Chat
1. Authenticate user with auth token
2. Retrieve user list: `GET /v3/users`
3. Select recipient and create conversation
4. Send message: `POST /v3/messages` with recipient UID
5. Listen for incoming messages via WebSocket
6. Display message with delivery/read receipts
7. Handle typing indicators in real-time
8. Support message editing and deletion

### Creating a Group Chat
1. Create group: `POST /v3/groups` with group name and type
2. Add members: `POST /v3/group_members` with user UIDs
3. Set group owner and member roles
4. Send group message: `POST /v3/messages` with group GUID
5. Manage group settings (description, icon, etc.)
6. Handle member join/leave events via webhooks
7. Support group moderation and member removal

### Implementing Voice/Video Calling
1. Initialize calling SDK with AppID
2. Request call permissions from user
3. Initiate call: `CometChat.initiateCall(callSettings)`
4. Handle incoming call notifications
5. Accept/reject call with `CometChat.acceptCall()` or `CometChat.rejectCall()`
6. Manage call UI (video views, mute, speaker)
7. End call and log call history
8. Retrieve call logs: `GET /v3/calls`

### Setting Up Content Moderation
1. Create moderation rules: `POST /v3/moderation/rules`
2. Add keywords to block: `POST /v3/moderation/keywords`
3. Configure moderation action (block, flag, or allow)
4. Monitor flagged messages: `GET /v3/moderation/flagged_messages`
5. Review and approve/reject messages
6. Set up webhook for moderation events
7. Configure image moderation settings
8. Create custom moderation rules for specific content

### Configuring Push Notifications
1. Obtain FCM/APNs credentials from respective providers
2. Add provider credentials via REST API: `POST /v3/notifications/fcm_providers`
3. Create notification templates: `POST /v3/notifications/templates`
4. Register device tokens: `POST /v3/notifications/push_tokens`
5. Configure notification preferences per user
6. Set quiet hours and delivery windows
7. Test notification delivery
8. Monitor notification logs: `GET /v3/notifications/logs`

### Migrating from Legacy Chat System
1. Export data from legacy system (users, groups, messages)
2. Import users: `POST /v3/data_import/users`
3. Import groups: `POST /v3/data_import/groups`
4. Import group members: `POST /v3/data_import/members`
5. Import messages: `POST /v3/data_import/messages`
6. Verify data integrity
7. Update user auth tokens
8. Notify users of migration completion

### Deploying AI Agents
1. Create AI agent in CometChat Agent Builder or connect third-party agent
2. Configure agent instructions and knowledge base
3. Set up agent tools and actions
4. Embed agent widget in application
5. Configure agent appearance and messaging
6. Test agent responses
7. Monitor agent performance and conversations
8. Update agent knowledge and rules as needed

## Troubleshooting & Validation
- **Auth errors**: Confirm correct AppID/region and that auth token matches UID; REST calls must use server-side API key, not client keys.
- **Real-time not connecting**: Check WebSocket reachability, ensure correct region, and verify network allows wss on required ports.
- **Push issues**: Validate FCM/APNs credentials, confirm device token registration, and inspect `/v3/notifications/logs`.
- **Calls failing**: Ensure camera/mic permissions, check TURN/ICE config if on restrictive networks, and verify appID/region match between caller/callee.
- **Moderation**: If messages are blocked, review moderation rules and flagged message logs; adjust action from block to flag if needed.
- **Rate limits**: Check response headers for limit info; back off with exponential retry.
- **Quick validation**: Send a test message via REST, read it via SDK, verify receipts/typing indicators, place a 1:1 test call, then send a test push.

## Security & Compliance Guardrails
- Keep API key server-side; never expose REST API key in client apps.
- Use region-specific endpoints to satisfy data residency (US/EU/IN).
- Enable least-privilege roles; use scoped auth tokens where possible.
- For healthcare/enterprise, confirm BAA/SOC2/ISO needs before go-live; enable message retention and audit logs.
- Mask PII in logs; enable data masking and profanity/XSS filters as defaults for new launches.

## Reference Map
- Main docs index (for deep links): https://www.cometchat.com/docs/llms.txt
- REST API overview: `/docs/rest/v3/` (via index above)
- JS SDK and UI Kits: `/docs/javascript/` and `/docs/ui-kits/`
- React Native: `/docs/react-native/`
- iOS: `/docs/ios/`
- Android: `/docs/android/`
- Notifications: `/docs/notifications/`
- Moderation/AI/Extensions: `/docs/extensions/`

## Integration

CometChat integrates with numerous platforms and services:

- **Frontend Frameworks**: React, Vue, Angular, Next.js, Astro, React Router
- **Mobile Platforms**: iOS (Swift), Android (Kotlin/Java), React Native, Flutter, Ionic
- **Backend Languages**: JavaScript/Node.js, Python, PHP, Java, Go, Ruby
- **Customer Support**: Intercom, Chatwoot
- **Notifications**: SendGrid (email), Twilio (SMS), FCM (Android), APNs (iOS)
- **Media**: Giphy, Tenor, Stipop stickers, api.video broadcasting
- **Security**: Virgil security, end-to-end encryption
- **Analytics**: Built-in analytics dashboard and API
- **Webhooks**: Custom integrations via HTTP POST events
- **Data Centers**: US, EU, IN regions for compliance

## Context

**Key Concepts**:
- **AppID**: Unique identifier for your CometChat application
- **API Key**: Authentication credential for REST API calls (keep secret)
- **Auth Token**: User-specific token for SDK authentication
- **UID**: Unique user identifier in your system
- **GUID**: Unique group identifier
- **WebSocket**: Real-time bidirectional connection for live messaging
- **Extensions**: Optional features that extend core functionality
- **Webhooks**: HTTP callbacks triggered by CometChat events
- **UI Kits**: Pre-built, customizable UI components
- **SDKs**: Software development kits for various platforms

**Rate Limits**: API calls are rate-limited per region. Check documentation for specific limits.

**Compliance**: CometChat meets HIPAA, GDPR, SOC 2, ISO 27001, and PIPEDA standards for enterprise use.

**Multi-Region**: Deploy in US, EU, or IN regions for data residency compliance.

**Scalability**: Platform supports millions of concurrent users with enterprise-grade infrastructure.

**Customization**: Extensive theming, component customization, and extension system for white-label solutions.

---

> For additional documentation and navigation, see: https://www.cometchat.com/docs/llms.txt
