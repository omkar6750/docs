# CometChat JavaScript SDK (Latest) — Agent Context

This file is a condensed, **AI-agent-friendly** reference for the *latest* JavaScript SDK docs in this repo.

## Scope (what this file is based on)

- Included sources: `sdk/javascript/*.mdx`
- Explicitly excluded (legacy): `sdk/javascript/2.0/**`, `sdk/javascript/3.0/**`

If a behavior/API isn’t mentioned here, consult the matching doc file in the **Doc Map** section at the end.

---

## Packages (what to install/import)

### Chat SDK

- NPM: `@cometchat/chat-sdk-javascript`
- Import:
  - `import { CometChat } from "@cometchat/chat-sdk-javascript";`

### Calls SDK

- NPM: `@cometchat/calls-sdk-javascript`
- Import:
  - `import { CometChatCalls } from "@cometchat/calls-sdk-javascript";`

---

## Credentials (Dashboard)

From the CometChat Dashboard, you typically need:

- **App ID**
- **Region**
- **Auth Key** (client-side login for dev/POC; also used for some “create user” client calls)

Related concept: **REST API Key** (server-side only; broader privileges than Auth Key).

---

## Initialization & Authentication (Chat SDK)

### Initialize

`CometChat.init(appID, appSettings)` must be called before using other Chat SDK APIs.

Common `AppSettingsBuilder` configuration:

- `.setRegion(region)` (required)
- Presence subscriptions:
  - `.subscribePresenceForAllUsers()`
  - `.subscribePresenceForRoles(roles: string[])`
  - `.subscribePresenceForFriends()`
- WebSocket management:
  - `.autoEstablishSocketConnection(true | false)` (default `true`)
- Dedicated/on‑prem:
  - `.overrideAdminHost(adminHost: string)`
  - `.overrideClientHost(clientHost: string)`

### Login

Two supported login styles:

- **Auth Key login (dev/POC):** `CometChat.login(UID, AUTH_KEY)`
- **Auth Token login (recommended for production):** `CometChat.login(AUTH_TOKEN)`

Session behavior:

- The SDK persists the logged-in session.
- Use `CometChat.getLoggedinUser()` to check for an existing session before calling `login()` again.

### Logout

- `CometChat.logout()`

### Login listener (optional)

- `CometChat.addLoginListener(listenerId, new CometChat.LoginListener({ ... }))`
- Events: `loginSuccess`, `loginFailure`, `logoutSuccess`, `logoutFailure`
- Remove: `CometChat.removeLoginListener(listenerId)`

---

## SSR (Next.js / Nuxt) note

Docs show dynamic importing the SDK on the client side (e.g., in `useEffect` / `componentDidMount`) to avoid SSR runtime issues.

---

## Core Concepts (IDs, entities, message taxonomy)

### IDs

- **UID** (User ID): alphanumeric with underscore/hyphen; **no spaces** or special punctuation.
- **GUID** (Group ID): same constraints as UID.

### Groups

- Types:
  - `CometChat.GROUP_TYPE.PUBLIC`
  - `CometChat.GROUP_TYPE.PASSWORD`
  - `CometChat.GROUP_TYPE.PRIVATE`
- Member scopes:
  - `CometChat.GROUP_MEMBER_SCOPE.ADMIN`
  - `CometChat.GROUP_MEMBER_SCOPE.MODERATOR`
  - `CometChat.GROUP_MEMBER_SCOPE.PARTICIPANT`

### Message categories & types

Messages belong to a category, then a type.

- `message` category types: `text`, `image`, `video`, `audio`, `file`
- `custom` category: developer-defined types (e.g., `"location"`)
- `interactive` category types: `form`, `card`, `customInteractive`
- `action` category: system events (member joined/left/kicked/etc, message edited/deleted)
- `call` category: call signaling messages (`audio` / `video`) with call status

---

## Constants / enums you’ll see a lot

- Receiver types: `CometChat.RECEIVER_TYPE.USER`, `CometChat.RECEIVER_TYPE.GROUP`
- Message types: `CometChat.MESSAGE_TYPE.TEXT|IMAGE|VIDEO|AUDIO|FILE|CUSTOM` (and interactive types via `InteractiveMessage`)
- Call types: `CometChat.CALL_TYPE.AUDIO`, `CometChat.CALL_TYPE.VIDEO`
- Call statuses (ringing flow): `CometChat.CALL_STATUS.REJECTED|CANCELLED|BUSY` (and others internally)
- Connection status: `connecting|connected|disconnected` (via `CometChat.getConnectionStatus()`)
- Moderation status: `CometChat.ModerationStatus.PENDING|APPROVED|DISAPPROVED`

---

## Real-time listeners (Chat SDK)

All listeners use a `listenerId` string. Avoid registering multiple listeners with the same ID unless you intend to replace/overwrite.

### UserListener (presence)

- Add: `CometChat.addUserListener(listenerId, new CometChat.UserListener({ ... }))`
- Callbacks:
  - `onUserOnline(user)`
  - `onUserOffline(user)`
- Remove: `CometChat.removeUserListener(listenerId)`

### GroupListener (group events)

- Add: `CometChat.addGroupListener(listenerId, new CometChat.GroupListener({ ... }))`
- Callbacks (common):
  - `onGroupMemberJoined(action, joinedUser, joinedGroup)`
  - `onGroupMemberLeft(action, leftUser, leftGroup)`
  - `onMemberAddedToGroup(action, userAdded, addedBy, addedTo)`
  - `onGroupMemberKicked(action, kickedUser, kickedBy, kickedFrom)`
  - `onGroupMemberBanned(action, bannedUser, bannedBy, bannedFrom)`
  - `onGroupMemberUnbanned(action, unbannedUser, unbannedBy, unbannedFrom)`
  - `onGroupMemberScopeChanged(action, changedUser, newScope, oldScope, changedGroup)`
- Remove: `CometChat.removeGroupListener(listenerId)`

### MessageListener (messages, receipts, typing, moderation, etc.)

- Add: `CometChat.addMessageListener(listenerId, new CometChat.MessageListener({ ... }))`
- Callbacks (common; see `sdk/javascript/all-real-time-listeners.mdx` for the full list):
  - Incoming messages: `onTextMessageReceived`, `onMediaMessageReceived`, `onCustomMessageReceived`, `onInteractiveMessageReceived`
  - Typing: `onTypingStarted`, `onTypingEnded`
  - Receipts: `onMessagesDelivered`, `onMessagesRead`, `onMessagesDeliveredToAll`, `onMessagesReadByAll`
  - Edits/deletes: `onMessageEdited`, `onMessageDeleted`
  - Transient: `onTransientMessageReceived`
  - Reactions: `onMessageReactionAdded`, `onMessageReactionRemoved`
  - Interactive goals: `onInteractionGoalCompleted`
  - Moderation: `onMessageModerated`
- Remove: `CometChat.removeMessageListener(listenerId)`

### CallListener (ringing/call signaling)

- Add: `CometChat.addCallListener(listenerId, new CometChat.CallListener({ ... }))`
- Callbacks:
  - `onIncomingCallReceived(call)`
  - `onOutgoingCallAccepted(call)`
  - `onOutgoingCallRejected(call)`
  - `onIncomingCallCancelled(call)`
  - `onCallEndedMessageReceived(call)`
- Remove: `CometChat.removeCallListener(listenerId)`

### ConnectionListener

- Add: `CometChat.addConnectionListener(listenerId, new CometChat.ConnectionListener({ ... }))`
- Callbacks:
  - `inConnecting()`
  - `onConnected()`
  - `onDisconnected()`
- Remove: not shown in these docs (most listeners follow a `removeXListener(listenerId)` pattern)
- Current status: `CometChat.getConnectionStatus()` → `connecting|connected|disconnected`

### AI assistant real-time events (AI Agents)

- Add: `CometChat.addAIAssistantListener(listenerId, { onAIAssistantEventReceived })`
- Remove: `CometChat.removeAIAssistantListener(listenerId)`

Events stream a “run lifecycle” (run start → tool call cycles → assistant text streams → run finished).

Agentic messages (persisted) arrive via `MessageListener`:

- `onAIAssistantMessageReceived(message)`
- `onAIToolResultReceived(message)`
- `onAIToolArgumentsReceived(message)`

---

## Messaging (send/receive/history/search)

### Send: Text messages

- Create: `new CometChat.TextMessage(receiverId, messageText, receiverType)`
- Send: `CometChat.sendMessage(textMessage)`
- Common setters:
  - `textMessage.setMetadata(object)`
  - `textMessage.setTags(string[])`
  - `textMessage.setQuotedMessageId(number)`
  - `textMessage.setParentMessageId(number)` (thread reply)

### Send: Media messages

- Create: `new CometChat.MediaMessage(receiverId, fileOrFiles, messageType, receiverType)`
  - `fileOrFiles` can be a single `File` or an array-like `FileList` (multi-attachment docs mention v3.0.9+)
  - `messageType` example: `CometChat.MESSAGE_TYPE.IMAGE|VIDEO|AUDIO|FILE`
- Send: `CometChat.sendMediaMessage(mediaMessage)`
- Common setters:
  - `mediaMessage.setMetadata(object)`
  - `mediaMessage.setCaption(string)`
  - `mediaMessage.setTags(string[])`
  - `mediaMessage.setQuotedMessageId(number)`
- Alternate “URL attachment” approach:
  - `new CometChat.Attachment({ name, extension, mimeType, url })`
  - `mediaMessage.setAttachment(attachment)`
  - (for multi: `mediaMessage.setAttachments(attachments)`)

### Send: Custom messages

- Create: `new CometChat.CustomMessage(receiverId, receiverType, customType, customData)`
- Send: `CometChat.sendCustomMessage(customMessage)`
- Common setters:
  - `customMessage.setSubtype(string)`
  - `customMessage.setTags(string[])`
  - `customMessage.setQuotedMessageId(number)`
  - `customMessage.shouldUpdateConversation(false)` (to prevent updating “last message”)

### Send: Interactive messages

- Create: `new CometChat.InteractiveMessage(receiverId, receiverType, interactiveType, interactiveData)`
- Send: `CometChat.sendInteractiveMessage(interactiveMessage)`
- Receive via `onInteractiveMessageReceived`
- Goal completion via `onInteractionGoalCompleted(receipt)`

### Send: Transient messages (real-time only; not stored)

- Create: `new CometChat.TransientMessage(receiverId, receiverType, data)`
- Send: `CometChat.sendTransientMessage(transientMessage)`
- Receive via `onTransientMessageReceived`

### Receive: real-time messages

Register a `MessageListener`. Note: senders do **not** receive their own message as a real-time event on the same device, but multi-device sessions can receive “self” events on other devices.

### Missed messages (offline delivery)

Use `CometChat.MessagesRequestBuilder()` with `.setMessageId(lastDeliveredId)` and call `fetchNext()` repeatedly.

- Get last delivered message id: `await CometChat.getLastDeliveredMessageId()`

### Unread messages

Use `MessagesRequestBuilder().setUnread(true)` and `fetchPrevious()`.

### Message history (pagination)

- Create a `MessagesRequest` using `MessagesRequestBuilder()`.
- Fetch:
  - `fetchPrevious()` → “older history” style pagination
  - `fetchNext()` → “newer” pagination in some flows

### Search messages

Docs use `MessagesRequestBuilder().setSearchKeyword("...")` (plan-gated features are called out in docs).

### Unread message counts

- `CometChat.getUnreadMessageCountForUser(UID, hideMessagesFromBlockedUsers)`
- `CometChat.getUnreadMessageCountForGroup(GUID, hideMessagesFromBlockedUsers)`
  - `hideMessagesFromBlockedUsers` is an optional boolean per docs

---

## Delivery & Read receipts

### Mark delivered

- `CometChat.markAsDelivered(messageId, receiverId, receiverType, senderId)` (or `CometChat.markAsDelivered(message)` with a message object)
- `CometChat.markConversationAsDelivered(conversationWith, conversationType)`

### Mark read

- `CometChat.markAsRead(messageId, receiverId, receiverType, senderId)`
- `CometChat.markConversationAsRead(conversationWith, conversationType)`

### Mark unread (revisit)

- `CometChat.markMessageAsUnread(message)` → returns updated `Conversation` with unread count

### Receive receipt events

Via `MessageListener`:

- `onMessagesDelivered(messageReceipt)`
- `onMessagesRead(messageReceipt)`
- Group-only:
  - `onMessagesDeliveredToAll(messageReceipt)`
  - `onMessagesReadByAll(messageReceipt)`

---

## Editing / deleting messages

### Edit

- Use `CometChat.editMessage(baseMessage)` (docs: only `TextMessage` and `CustomMessage`)
- Ensure the message has the correct `id` set (e.g., `textMessage.setId(messageId)`)
- Real-time event: `onMessageEdited(message)`
- Offline/history: edited messages have `editedAt`/`editedBy`; action messages may also appear

### Delete

- `CometChat.deleteMessage(messageId)`
- Real-time event: `onMessageDeleted(message)`
- Offline/history: deleted messages have `deletedAt`/`deletedBy`; action messages may also appear

---

## Threaded messages

- Send into a thread: set `parentMessageId` on outgoing message:
  - `textMessage.setParentMessageId(parentId)` (also for media/custom)
- Receive: check `message.getParentMessageId()` against the active thread.
- Fetch only thread replies: `MessagesRequestBuilder().setParentMessageId(parentId)`
- Hide thread replies in the main conversation feed: `MessagesRequestBuilder().hideReplies(true)`

---

## Mentions

- Mention syntax in text/captions: `<@uid:cometchat-uid-1>`
- Fetch mention metadata options on `MessagesRequestBuilder`:
  - `mentionsWithTagInfo(true)`
  - `mentionsWithBlockedInfo(true)`
- Advanced-search filters (plan-gated in docs):
  - `hasMentions(true)`
  - `setMentionedUIDs(["uid1", "uid2"])`

---

## Reactions

- Add: `CometChat.addReaction(messageId, emoji)`
- Remove: `CometChat.removeReaction(messageId, emoji)`
- Fetch reactions:
  - `new CometChat.ReactionRequestBuilder().setMessageId(messageId).setLimit(n)...build()`
  - Optional filter: `.setReaction("😊")`
  - Pagination: `fetchNext()`, `fetchPrevious()`
- Real-time:
  - `onMessageReactionAdded(event)`
  - `onMessageReactionRemoved(event)`
- Helpers:
  - `message.getReactions()` → list of `ReactionCount`
  - `reactionCount.getReactedByMe()` → boolean
  - `CometChat.updateMessageWithReactionInfo(message, reactionEvent, action)` where action is `CometChat.REACTION_ACTION.REACTION_ADDED|REACTION_REMOVED`

---

## Advanced message filtering (MessagesRequestBuilder)

Core filters (generally available):

- Scope:
  - `.setUID(uid)` / `.setGUID(guid)`
  - `.setLimit(n)` (max 100 per page)
- Pagination anchors:
  - `.setMessageId(id)`
  - `.setTimestamp(unixSeconds)`
- Unread:
  - `.setUnread(true)`
- Blocked relationships:
  - `.hideMessagesFromBlockedUsers(true)`
- Update sync:
  - `.setUpdatedAfter(unixSeconds)`
  - `.updatesOnly(true)` (requires `setUpdatedAfter`)
- Categories/types:
  - `.setCategories(["message", "custom", ...])`
  - `.setTypes(["text", "image", ...])`
- Threads:
  - `.setParentMessageId(parentId)`
  - `.hideReplies(true)`
- Visibility:
  - `.hideDeletedMessages(true)`
  - `.hideQuotedMessages(true)`
- Tags:
  - `.setTags(["tag1"])`
  - `.withTags(true)`

Advanced Search filters (docs call these out as plan-gated via “Conversation & Advanced Search”):

- `.hasLinks(true)`
- `.hasAttachments(true)`
- `.hasReactions(true)`
- `.hasMentions(true)`
- `.setMentionedUIDs(["uid"])`
- `.setAttachmentTypes([CometChat.AttachmentType.IMAGE, ...])`

---

## Moderation & safety

### AI Moderation

- Moderated message types (per docs): text, image, video.
- On send, moderation status can be `PENDING`.
- Listen for the final decision via `MessageListener.onMessageModerated(message)`.
- Status values:
  - `CometChat.ModerationStatus.PENDING`
  - `CometChat.ModerationStatus.APPROVED`
  - `CometChat.ModerationStatus.DISAPPROVED`

### Flagging messages (user reports)

- Get reasons: `CometChat.getFlagReasons()`
- Flag: `CometChat.flagMessage(messageId, { reasonId, remark })` (`remark` is optional)
- Flagged messages appear in Dashboard under Moderation → Flagged Messages.

---

## Conversations (recent chats)

Use `ConversationsRequestBuilder` → `build()` → `fetchNext()`.

Filters mentioned in docs:

- `.setLimit(n)`
- `.setConversationType("user" | "group")`
- `.withUserAndGroupTags(true)`
- `.setUserTags([...])`
- `.setGroupTags([...])`
- `.withTags(true)`
- `.setTags([...])`
- `.setIncludeBlockedUsers(true)`
- `.setWithBlockedInfo(true)`
- Search keyword support is documented as plan-gated (“Conversation & Advanced Search”).

---

## Users

### Create/update users (client-side; typically server-side in production)

- Create: `CometChat.createUser(user, authKey)`
- Update: `CometChat.updateUser(user, authKey)`
- Update current user (no authKey): `CometChat.updateCurrentUserDetails(user)`
- Delete user: REST API only (per docs)

### Retrieve logged-in user

- `CometChat.getLoggedinUser()`

### List users (UsersRequestBuilder)

Common filters:

- `.setLimit(n)`
- `.setSearchKeyword(keyword)`
- `.searchIn(["uid", "name"])`
- `.setStatus(CometChat.USER_STATUS.ONLINE | OFFLINE)`
- `.hideBlockedUsers(true)`
- `.setRoles(["role1", "role2"])`
- `.friendsOnly(true)`
- `.setTags(["tag1"])`
- `.withTags(true)`
- `.setUIDs(["uid1", "uid2"])` (max 25)
- `.sortBy("name" | ...)` (docs: default sort is `status => name => UID`)

### Blocking

- Block: `CometChat.blockUsers(["uid1", "uid2"])`
- Unblock: `CometChat.unblockUsers(["uid1", "uid2"])`
- Fetch blocked lists:
  - `BlockedUsersRequestBuilder().setLimit(n).setSearchKeyword(...).setDirection(...)...build().fetchNext()`
  - Directions:
    - `CometChat.BlockedUsersRequest.directions.BLOCKED_BY_ME`
    - `CometChat.BlockedUsersRequest.directions.HAS_BLOCKED_ME`
    - `CometChat.BlockedUsersRequest.directions.BOTH`

---

## Presence

Presence subscription is configured at `init()` time via `AppSettingsBuilder`:

- `subscribePresenceForAllUsers()`
- `subscribePresenceForRoles(roles)`
- `subscribePresenceForFriends()`

Presence events arrive via `UserListener`:

- `onUserOnline(user)`
- `onUserOffline(user)`

User objects include:

- `status` (`online`/`offline`)
- `lastActiveAt` (timestamp when offline)

---

## Groups

### Create

- `new CometChat.Group(GUID, name, groupType, password)` (password is required for password groups; otherwise pass `""`)
- `CometChat.createGroup(group)`
- Create + add members + ban list:
  - `CometChat.createGroupWithMembers(group, members: GroupMember[], banMembers: string[])`
  - `new CometChat.GroupMember(UID, scope)`

### Retrieve groups

- Typical builder usage: `new CometChat.GroupsRequestBuilder().setLimit(n).setSearchKeyword(...).joinedOnly(true).setTags([...]).withTags(true).build()`
- Pagination: `fetchNext()`
- Get group details: `CometChat.getGroup(GUID)`
- Online member count: `CometChat.getOnlineGroupMemberCount([guid1, guid2])`

### Join / leave

- Join: `CometChat.joinGroup(GUID, groupType, password)` (password required only for password groups; pass `""` or omit for public/private)
- Leave: `CometChat.leaveGroup(GUID)`
- Group object includes `hasJoined` to determine membership state.

### Members

- Add members: `CometChat.addMembersToGroup(GUID, members: GroupMember[], bannedMembers: string[])`
- List members:
  - `new CometChat.GroupMembersRequestBuilder(GUID).setLimit(n).setSearchKeyword(...).setScopes([...]).setStatus(ONLINE|OFFLINE)...build().fetchNext()`
- Kick/ban/unban:
  - `CometChat.kickGroupMember(GUID, UID)`
  - `CometChat.banGroupMember(GUID, UID)`
  - `CometChat.unbanGroupMember(GUID, UID)`
- List banned members:
  - `new CometChat.BannedMembersRequestBuilder(GUID).setLimit(n).setSearchKeyword(...).build().fetchNext()`
- Change scope:
  - `CometChat.updateGroupMemberScope(GUID, UID, scope)`

### Update / delete / transfer ownership

- Update: `CometChat.updateGroup(group)`
- Delete: `CometChat.deleteGroup(GUID)` (admin required)
- Transfer ownership: `CometChat.transferGroupOwnership(GUID, newOwnerUID)` (owner-only)

### Group events (real-time)

Use `GroupListener` callbacks:

- Joined/left: `onGroupMemberJoined`, `onGroupMemberLeft`
- Added: `onMemberAddedToGroup`
- Kick/ban/unban: `onGroupMemberKicked`, `onGroupMemberBanned`, `onGroupMemberUnbanned`
- Scope changes: `onGroupMemberScopeChanged`

Offline/history: many group membership changes appear as `Action` messages in message history.

---

## Calling (Chat SDK + Calls SDK)

### Choose implementation

Docs present three approaches:

1. **Ringing (complete flow):** signaling + UI for incoming/outgoing calls (`sdk/javascript/default-call.mdx`)
2. **Call Session (session management):** token + session join (`sdk/javascript/direct-call.mdx`)
3. **Standalone Calling:** Calls SDK only, auth token via REST (`sdk/javascript/standalone-calling.mdx`)

### Calls SDK initialization

- Build `CallAppSettings` (per docs, builder supports at least `appId` and `region`; `host` override is mentioned for dedicated deployments):
  - `new CometChatCalls.CallAppSettingsBuilder().setAppId(appID).setRegion(region)...build()`
- Init: `CometChatCalls.init(callAppSetting)`

### Ringing flow (Chat SDK)

- Initiate:
  - `new CometChat.Call(receiverID, callType, receiverType)`
  - `CometChat.initiateCall(call)`
- Listen: `CometChat.addCallListener(...)`
- Accept: `CometChat.acceptCall(sessionId)`
- Reject incoming: `CometChat.rejectCall(sessionId, CometChat.CALL_STATUS.REJECTED)`
- Cancel outgoing: `CometChat.rejectCall(sessionId, CometChat.CALL_STATUS.CANCELLED)`
- Busy handling: `CometChat.rejectCall(sessionId, CometChat.CALL_STATUS.BUSY)`

After acceptance, both participants start a **call session** using Calls SDK (`startSession()`).

### Call session (Calls SDK)

1. Generate a call token:
   - `CometChatCalls.generateToken(sessionId, authToken)`
   - For ringing + Chat SDK: `authToken` is usually `CometChat.getLoggedinUser().getAuthToken()`
2. Build call settings:
   - `new CometChatCalls.CallSettingsBuilder()...build()`
3. Start:
   - `CometChatCalls.startSession(callToken, callSettings, htmlElement)`

#### Ongoing call listener (in-session)

Use either:

- `new CometChatCalls.OngoingCallListener({ ... })` and pass via `CallSettingsBuilder.setCallListener(listener)`
- or `CometChatCalls.addCallEventListener(listenerId, { ... })` (multiple listeners supported)

Common callbacks:

- `onUserJoined`, `onUserLeft`, `onUserListUpdated`
- `onCallEnded`, `onCallEndButtonPressed`
- `onScreenShareStarted`, `onScreenShareStopped`
- `onMediaDeviceListUpdated`, `onUserMuted`
- `onCallSwitchedToVideo`
- `onSessionTimeout` (docs: v4.1.0+)
- `onRecordingStarted`, `onRecordingStopped` (recording)
- `onError`

### Ending a call session (important)

Ringing flow (Chat SDK + Calls SDK):

- User who ends the call:
  - Call `CometChat.endCall(sessionId)` (notifies other participant(s))
  - Then `CometChat.clearActiveCall()` and `CometChatCalls.endSession()` (releases resources)
- Remote participant:
  - Handle `onCallEnded()` → `CometChat.clearActiveCall()` then `CometChatCalls.endSession()`

Session-only flow:

- Just call `CometChatCalls.endSession()`

### In-session control methods (Calls SDK)

Available during an active session:

- `CometChatCalls.switchCamera()` (mobile browsers only; docs: v4.2.0+)
- `CometChatCalls.muteAudio(true|false)`
- `CometChatCalls.pauseVideo(true|false)`
- `CometChatCalls.startScreenShare()`, `CometChatCalls.stopScreenShare()`
- `CometChatCalls.setMode(CometChat.CALL_MODE.DEFAULT|SPOTLIGHT)`
- Device discovery:
  - `CometChatCalls.getAudioInputDevices()`
  - `CometChatCalls.getAudioOutputDevices()`
  - `CometChatCalls.getVideoInputDevices()`
- Device selection:
  - `CometChatCalls.setAudioInputDevice(deviceId)`
  - `CometChatCalls.setAudioOutputDevice(deviceId)`
  - `CometChatCalls.setVideoInputDevice(deviceId)`
- Upgrade audio → video:
  - `CometChatCalls.switchToVideoCall()`
- End:
  - `CometChatCalls.endSession()`

---

## Calling features (selected)

### Recording (beta)

- Listener callbacks: `onRecordingStarted`, `onRecordingStopped`
- CallSettingsBuilder options:
  - `showRecordingButton(true|false)`
  - `startRecordingOnCallStart(true|false)`
- Manual control:
  - `CometChatCalls.startRecording()`
  - `CometChatCalls.stopRecording()`
- Recordings are accessed via Dashboard (per docs).

### Virtual background

- CallSettingsBuilder:
  - `showVirtualBackgroundSetting(true|false)`
  - `setVirtualBackground(virtualBackgroundConfig)`
- CallController (runtime controls):
  - `CometChat.CallController.getInstance().openVirtualBackground()`
  - `callController.setBackgroundBlur(level)`
  - `callController.setBackgroundImage(urlOrFile)`
- VirtualBackground config supports:
  - allow/enforce blur
  - allow user images
  - default images + custom image set

### Video view customization

- Build `MainVideoContainerSetting` and pass to `CallSettingsBuilder.setMainVideoContainerSetting(...)`
- Options include:
  - aspect ratio (`CONTAIN` / `COVER`)
  - full screen button params
  - name label params
  - network label params

### Custom CSS

Docs list common CSS classes for styling the call UI, including:

- `cc-main-container`
- `cc-bottom-buttons-container`
- `cc-bottom-buttons-icon-container`
- `cc-name-label`
- `cc-video-container`
- Bottom button variants such as:
  - `cc-audio-icon-container`, `cc-audio-icon-container-muted`
  - `cc-video-icon-container`, `cc-video-icon-container-muted`
  - `cc-screen-share-icon-container`
  - `cc-switch-video-icon-container`
  - `cc-end-call-icon-container`

### Presenter mode

- Join presentation: `CometChatCalls.joinPresentation(callToken, presenterSettings, htmlElement)`
- Configure with `CometChatCalls.PresenterSettingsBuilder()`:
  - `setIsPresenter(true|false)` (presenter vs viewer)
  - `enableDefaultLayout(true|false)`
  - listener registration similar to regular calls via call event listeners

### Call logs

- `new CometChatCalls.CallLogRequestBuilder()...build()`
- Filters include:
  - `setLimit`, `setAuthToken`
  - `setCallCategory("call"|"meet")`
  - `setCallType("audio"|"video")`
  - `setCallStatus("ongoing"|"busy"|...)`
  - `setHasRecording(boolean)`
  - `setCallDirection("incoming"|"outgoing")`
  - `setUid(uid)`, `setGuid(guid)`
- Pagination: `fetchNext()`, `fetchPrevious()`
- Call details: `CometChatCalls.getCallDetails(sessionId, authToken)`

### Session timeout

Docs describe an idle timeout flow (default 180 seconds alone in session) and an `onSessionTimeout` event.

CallSettingsBuilder includes `setIdleTimeoutPeriod(seconds)` (docs: v4.1.0+).

---

## Manual WebSocket management (advanced)

If you want to manage the Chat SDK WebSocket yourself:

1. Set `.autoEstablishSocketConnection(false)` in `AppSettingsBuilder` during `init()`.
2. After login, call:
   - `CometChat.connect()` to open the WebSocket
   - `CometChat.disconnect()` to close it

Use `ConnectionListener` or `CometChat.getConnectionStatus()` to track connection state.

---

## Rate limits (high level)

Docs mention:

- Core operations: `10,000` requests/minute (cumulative)
- Standard operations: `20,000` requests/minute (cumulative)

On limit exceed, API returns `429` with headers like `Retry-After` and rate-limit counters.

---

## Redirect pages inside `sdk/javascript`

Some files are simple redirects to other docs sections (they contain `url:` frontmatter only):

- `sdk/javascript/ai-chatbots-overview.mdx` → `/ai-chatbots/overview`
- `sdk/javascript/ai-user-copilot-overview.mdx` → `/fundamentals/ai-user-copilot/overview`
- `sdk/javascript/extensions-overview.mdx` → `/fundamentals/extensions-overview`
- `sdk/javascript/webhooks-overview.mdx` → `/fundamentals/webhooks-overview`
- `sdk/javascript/react-overview.mdx` → `/ui-kit/react/overview`
- `sdk/javascript/vue-overview.mdx` → `/ui-kit/vue/overview`
- `sdk/javascript/angular-overview.mdx` → `/ui-kit/angular/overview`
- `sdk/javascript/changelog.mdx` → GitHub releases URL

---

## Doc map (latest JavaScript SDK docs in this folder)

### Setup & concepts

- `sdk/javascript/overview.mdx` — end-to-end integration overview (incl. SSR examples)
- `sdk/javascript/setup-sdk.mdx` — install + initialize Chat SDK (v4)
- `sdk/javascript/key-concepts.mdx` — dashboard/keys, IDs, groups, message categories
- `sdk/javascript/upgrading-from-v3.mdx` — v3 → v4 upgrade notes
- `sdk/javascript/rate-limits.mdx` — rate-limit behavior and headers

### Authentication & sessions

- `sdk/javascript/authentication-overview.mdx` — login via Auth Key/Auth Token, logout, session notes
- `sdk/javascript/login-listener.mdx` — login/logout real-time events
- `sdk/javascript/connection-status.mdx` — connection listener + status getter
- `sdk/javascript/managing-web-sockets-connections-manually.mdx` — manual `connect()`/`disconnect()` flow

### Users & presence

- `sdk/javascript/users-overview.mdx` — users entry overview
- `sdk/javascript/user-management.mdx` — create/update user APIs + `User` fields
- `sdk/javascript/retrieve-users.mdx` — `UsersRequestBuilder` filters + pagination
- `sdk/javascript/block-users.mdx` — block/unblock + blocked list request builder
- `sdk/javascript/user-presence.mdx` — presence subscriptions + `UserListener`

### Groups

- `sdk/javascript/groups-overview.mdx` — groups overview
- `sdk/javascript/create-group.mdx` — create group (+ members)
- `sdk/javascript/retrieve-groups.mdx` — groups list request builder + `getGroup()` + online count
- `sdk/javascript/retrieve-group-members.mdx` — group members request builder
- `sdk/javascript/group-add-members.mdx` — add members + real-time “added” events
- `sdk/javascript/group-kick-ban-members.mdx` — kick/ban/unban + banned list + events
- `sdk/javascript/group-change-member-scope.mdx` — scope changes + events
- `sdk/javascript/join-group.mdx` — join + events
- `sdk/javascript/leave-group.mdx` — leave + events
- `sdk/javascript/update-group.mdx` — update group
- `sdk/javascript/delete-group.mdx` — delete group
- `sdk/javascript/transfer-group-ownership.mdx` — transfer ownership

### Messaging (core)

- `sdk/javascript/messaging-overview.mdx` — messaging entry overview
- `sdk/javascript/send-message.mdx` — text/media/custom + metadata/tags/quoted + attachments
- `sdk/javascript/receive-message.mdx` — listener + missed/unread/history/search + unread counts
- `sdk/javascript/delivery-read-receipts.mdx` — mark delivered/read/unread + receipt events
- `sdk/javascript/typing-indicators.mdx` — typing indicator send/receive
- `sdk/javascript/message-structure-and-hierarchy.mdx` — categories/types diagram + definitions
- `sdk/javascript/additional-message-filtering.mdx` — `MessagesRequestBuilder` filters (incl. plan-gated)
- `sdk/javascript/retrieve-conversations.mdx` — `ConversationsRequestBuilder` for recent chat list
- `sdk/javascript/delete-conversation.mdx` — delete conversation for current user

### Messaging (advanced UX features)

- `sdk/javascript/edit-message.mdx` — edit message + events/history notes
- `sdk/javascript/delete-message.mdx` — delete message + events/history notes
- `sdk/javascript/threaded-messages.mdx` — threads via `parentMessageId` + hide replies
- `sdk/javascript/transient-messages.mdx` — transient messages (non-persisted)
- `sdk/javascript/interactive-messages.mdx` — interactive message model + listeners
- `sdk/javascript/mentions.mdx` — mention syntax + fetch options
- `sdk/javascript/reactions.mdx` — add/remove/fetch reactions + events

### AI & moderation

- `sdk/javascript/ai-agents.mdx` — AI agent run lifecycle + assistant/tool message listeners
- `sdk/javascript/ai-moderation.mdx` — moderation status + `onMessageModerated`
- `sdk/javascript/flag-message.mdx` — report flow + reasons + flag API

### Calling

- `sdk/javascript/calling-overview.mdx` — calling options + feature list
- `sdk/javascript/calling-setup.mdx` — Calls SDK setup + init
- `sdk/javascript/default-call.mdx` — ringing flow (initiate/accept/reject/cancel)
- `sdk/javascript/direct-call.mdx` — session flow (token/start/end) + in-call methods
- `sdk/javascript/standalone-calling.mdx` — Calls SDK-only flow + auth via REST
- `sdk/javascript/recording.mdx` — recording feature
- `sdk/javascript/virtual-background.mdx` — virtual background feature
- `sdk/javascript/presenter-mode.mdx` — presenter mode feature
- `sdk/javascript/video-view-customisation.mdx` — main video view customization
- `sdk/javascript/custom-css.mdx` — call UI CSS hooks
- `sdk/javascript/call-logs.mdx` — call history fetch/filter + call details
- `sdk/javascript/session-timeout.mdx` — idle session timeout behavior + event

### Resources

- `sdk/javascript/resources-overview.mdx` — entry point to helper docs
- `sdk/javascript/all-real-time-listeners.mdx` — consolidated listener reference

---

## Known documentation issues (for future cleanup)

These are **documentation inconsistencies** noticed while building this context (verify against the SDK if you plan to patch them):

- `sdk/javascript/retrieve-users.mdx` text mentions `getLoggedInUser()` but code/examples across docs frequently use `getLoggedinUser()`.
- `sdk/javascript/reactions.mdx` uses `removeMessageReactionListener()` in one TypeScript snippet, but other docs remove listeners via `removeMessageListener(listenerId)`.
- `sdk/javascript/additional-message-filtering.mdx` contains multiple copy/paste mistakes in some snippets (e.g., `.setGUID(UID)` in “user” examples, `.withTags(tags)` where a boolean is expected).
- `sdk/javascript/call-logs.mdx` has a stray “23b application” phrase (likely a typo).
- `sdk/javascript/presenter-mode.mdx` contains copy/paste references to other platforms (React Native/Android) while being in JavaScript docs.
